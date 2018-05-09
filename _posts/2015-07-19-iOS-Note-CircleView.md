---
layout: post
title:  "iOS开发笔记-自定义画图·过度色圆环"
date:   2015-07-19 22:34:00 +0800
tags: iOS
category: Work
---

~iOS笔记

## 项目背景：

从iOS开发转PM了，不写代码就生手了，将设计的动画自己开发出来练练手哈哈。

需求是：从0画4圈圆环，每圈都是不同的过度色，0-1-2-3-4的颜色过渡~

***

之前使用Quartz2D画过一些简单的图，但是我并没有找到可以画过渡色的画笔~然而我的需求是画一个过渡色的圆环并且超过一圈时第二圈的颜色也需要新的过渡~

下面是我尝试过的几种方法~

### 方法一：先做单色圆环，再换颜色画第二圈（不符合需求）

用最简单的在view的drawRect方法里画圆，每个圆的画笔颜色换掉即可~

### 方法二：彩色圆环图片+橡皮擦画笔涂擦

UI提供了三圈过度色的圆环图片，在图片上盖一层不透明的view然后再用透明画笔涂擦（类似橡皮擦效果），达到看起来是过度色的效果~可是只能达到只画一圈的效果。

### 方法三：改造下载的第三方库

（在code4app.com里找的MulticolorLayerDemo）

* 里面使用的是CAGradientLayer对象来处理颜色渐变，同时使用**UIBezierPath对象生成圆环路径**然后用**CAShapeLayer对象生成圆环路径的layer**；
* 我将里面的函数改造并抽取封装，就可以达到变态需求的效果~（这个过程并不简单，因为自己是产品经理&&iOS程序员，自己作出来的变态需求，就自己实现！TAT）


***

此三个方法可以看做三个不同的demo

## Demo1.画单色圆环，并且第二圈换色

![Demo1](http://upload-images.jianshu.io/upload_images/559895-2cd02d4cffb6688e.png)

虽然与该产品需求不符合，但是也有其他需求能用得上~代码总是宝贵的！

### 项目逻辑结构：

1. viewController ：circleView展示圆环，slider调节来显示圆环弧度 ；
2. circleView ：实现drawRect: 方法，方法内部调用UIView(Draw)分类的方法画图；
3. UIView(Draw) ：将画圆、画圆弧、画线、画矩形等方法都抽取到这里，只要传递参数和CGContextRef对象调用即可根据参数画图。

### 主要代码：

**viewController ：**共5行代码，直接使用circleView即可,然后将slider的值传递给circleView里
创建circleView，拖动UIView到storyboard的控制器面板上，然后将该view的类改为CircleView,连接IBoutlet.

slider的valueChange关联方法：

```
- (IBAction)changeSlider:(UISlider *)sender {
    //circleView 的属性，弧度比例，kTotalTime = 2,表示两圈
    self.circleView.anglePercent = sender.value/kTotalTime;
    NSLog(@"value--%f,anglePercent--%f",sender.value,self.circleView.anglePercent);
}

```

**circleView ：**主要是在drawRect:方法里调用工具类的画圆方法，并且在.h文件里提供property给外界传递参数

属性：

```
/**
 *  半径
 */
@property (assign,nonatomic) NSInteger radius;
/**
 *  弧度比
 */
@property (assign,nonatomic) double anglePercent;
```

drawRect:方法


```
/**
 *  在view第一次显示到屏幕上的时候会调用一次
 */
- (void)drawRect:(CGRect)rect {
    //只有在这个方法获取的图形上下文才有效
    CGContextRef ctx = UIGraphicsGetCurrentContext();

    //多个圆弧
    [self drawMoreArc:ctx rect:rect radius:kRadius anglePercent:self.anglePercent lineWidth:kLineWidth redValue:kRedValue greenValue:kGreenValue blueValue:kBlueValue colorValue:self.colorValue colorMultiple:kColorMultiple];
    
    //    //画线
    //    [self drawLine:ctx rect:rect];画线方法
    //    //画圆方法
    //    [self drawCircle:ctx radius:kRadius*2 rect:rect];
    //    //画圆弧
    //    [self drawArc:ctx radius:kRadius anglePercent:0.5 rect:rect];
}

```

重写set方法：


```
-(void)setRadius:(NSInteger)radius{
    _radius = radius;
    [self setNeedsDisplay];
}

-(void)setAnglePercent:(double)anglePercent{
    _anglePercent = anglePercent;
    [self setNeedsDisplay];
}

```

***

**UIView (Draw) ：**是UIView的分类，封装了很多画图方法，可以直接用,这里只贴画圆的

```
/**
 *  画多重圆弧
 *
 *  @param ctx          上下文
 *  @param r            半径
 *  @param anglePercent 角度
 *  @param red          红色值
 *  @param green        绿色值
 *  @param blue         蓝色值
 *  @param colorValue   颜色过渡值
 *  @param multiple     颜色过渡倍数
 */
-(void)drawMoreArc:(CGContextRef)ctx rect:(CGRect)rect radius:(NSInteger)r anglePercent:(double)anglePercent lineWidth:(float)width redValue:(double)red greenValue:(double)green blueValue:(double)blue colorValue:(double)colorValue colorMultiple:(NSInteger)multiple{
    //1.第一个圆
    CGContextAddArc(ctx, rect.size.width/2, rect.size.height/2, r, -M_PI_2, -M_PI_2+anglePercent*M_PI*2, 0);
    
    CGContextSetLineWidth(ctx, width);
    CGContextSetLineCap(ctx, kCGLineCapRound);
    // 设置颜色
    CGContextSetRGBStrokeColor(ctx, red/255.0, green/255.0, blue/255.0, 1);
    // 渲染一次
    CGContextStrokePath(ctx);

    //2.弧度比大于1，第二个圆
    if (anglePercent>=1) {
        
        CGContextAddArc(ctx, rect.size.width/2, rect.size.height/2, r, -M_PI_2,  -M_PI_2+(anglePercent-1)*M_PI*2, 0);
        
        CGContextSetLineWidth(ctx, width+2);
        CGContextSetLineCap(ctx, kCGLineCapRound);
        // 设置画笔颜色
        if(colorValue>=kFirstRound&&colorValue<kSecondRound) {
            CGContextSetRGBStrokeColor(ctx, red, (green-(colorValue-kFirstRound)*multiple)/255.0, blue/255.0, 1);
        }else{
            CGContextSetRGBStrokeColor(ctx, red, (green-12*multiple)/255.0, blue/255.0, 1);
        }

    }

    // 3.显示所绘制的东西
    CGContextStrokePath(ctx);
}

```


***

## Demo2.橡皮擦效果（透明画笔）

![第一圈](http://upload-images.jianshu.io/upload_images/559895-c8e86ecc03cce70c.png)

![第二圈](http://upload-images.jianshu.io/upload_images/559895-3208542d0fdd2f9a.png)

第一圈绿色，第二圈从绿色过渡到紫色~
勉强达到效果，但是只能画一圈，在第三圈开始就没法继续变色。

### 项目逻辑结构：
在view的下面放一个UI提供好的圆环图片，设置hidden=YES，第一圈结束，将画笔颜色变成透明色擦掉原来画好的路径，并且将底下图片的hidden改成NO，这样就能像橡皮擦一样慢慢将图片展示出来~


### 主要代码：
工具类的画圆方法，主要是画笔的更改：//橡皮擦功能
        CGContextSetBlendMode(ctx, kCGBlendModeCopy);

```
/**
 *  画多重圆弧
 *
 *  @param ctx          上下文
 *  @param r            半径
 *  @param anglePercent 角度
 *  @param red          红色值
 *  @param green        绿色值
 *  @param blue         蓝瑟值
 *  @param colorValue   颜色过渡值
 *  @param multiple     颜色过渡倍数
 */
-(void)drawMoreArc:(CGContextRef)ctx rect:(CGRect)rect radius:(NSInteger)r anglePercent:(double)anglePercent lineWidth:(float)width redValue:(double)red greenValue:(double)green blueValue:(double)blue colorValue:(double)colorValue colorMultiple:(NSInteger)multiple{
        //1.第一个圆
        CGContextAddArc(ctx, rect.size.width/2, rect.size.height/2, r, -M_PI_2, -M_PI_2+anglePercent*M_PI*2, 0);
        
        CGContextSetLineWidth(ctx, width);
//        CGContextSetLineCap(ctx, kCGLineCapRound);
        // 设置颜色
        CGContextSetRGBStrokeColor(ctx, red/255.0, green/255.0, blue/255.0, 1);
        // 渲染一次
        CGContextStrokePath(ctx);
    if (anglePercent>=1) {
        //橡皮擦功能
        CGContextSetBlendMode(ctx, kCGBlendModeCopy);
        
        //第二个圆
        CGContextAddArc(ctx, rect.size.width/2, rect.size.height/2, r, -M_PI_2,  -M_PI_2+(anglePercent-1)*M_PI*2, 0);
        
        CGContextSetLineWidth(ctx, width);
//        CGContextSetLineCap(ctx, kCGLineCapRound);
        // 设置颜色
        CGContextSetRGBStrokeColor(ctx, red, (green-12*multiple)/255.0, blue/255.0, 0);

    }
    // 3.显示所绘制的东西
    CGContextStrokePath(ctx);
    
}

```

***

## Demo3.改第三方库（综合）

这个是目前解决当前需求的办法：将别人封装好的MulticolorView更换参数，然后传递弧度比，就能创建无数圈的圆环~并且每圈都是过度色~

在.h文件声明创建圆环方法，内部实现：

```
- (CAShapeLayer *)produceCircleShapeLayer:(double)angle
{
    // 生产出一个圆的路径 -M_PI_2 -M_PI_2+angle*M_PI*2
    CGPoint circleCenter = CGPointMake(CGRectGetMidX(self.bounds), CGRectGetMidY(self.bounds));
    CGFloat circleRadius = self.bounds.size.width/2.0 - 10;
    if (angle>2) {
        angle-=2;
    }else if (angle>1){
        angle-=1;
    }
    UIBezierPath *circlePath = [UIBezierPath bezierPathWithArcCenter:circleCenter
                                                              radius:circleRadius
                                                          startAngle:-M_PI_2+angle*M_PI*2
                                                            endAngle:-M_PI_2
                                                           clockwise:NO];
    
    // 生产出一个圆形路径的Layer
    CAShapeLayer *shapeLayer = [CAShapeLayer layer];
    shapeLayer.path = circlePath.CGPath;
    shapeLayer.strokeColor = [UIColor lightGrayColor].CGColor;
    shapeLayer.fillColor = [[UIColor clearColor] CGColor];
    shapeLayer.lineWidth = 7;
//    shapeLayer.lineCap = @"round";
    
    // 可以设置出圆的完整性
    shapeLayer.strokeStart = 0;
    shapeLayer.strokeEnd = 1.0;
    
    return shapeLayer;
}

```

### 重点来了~过渡色到底该怎么画呢？

首先要简单了解一个类：CAGradientLayer（CAGradientLayer是处理颜色渐变的一个图层）。

具体详解参照官方文档或者百度等,这个是它的简介： <http://blog.csdn.net/ch_soft/article/details/7534542>。


```
- (void)setupMulticolor:(double)anglePercent
{

    //iOS实现一个颜色渐变的弧形进度条：为什么要这么折腾？CAShapeLayer不能顺着弧线进行渐变只能指定两个点之间进行渐变
    
    //所以...左半圆
    NSArray * colors0 = [NSArray array];
    if (anglePercent <= 0.01) {//第0圈
        colors0 = @[(__bridge id)[UIColor colorWithRGBHex:0xf9f9f9].CGColor,
                   (__bridge id)[UIColor colorWithRGBHex:0xf9f9f9].CGColor];
    }
    else if (anglePercent>0.01&&anglePercent<=1) {//第1圈
        colors0 = @[(__bridge id)[UIColor colorWithRGBHex:0x00c7b7].CGColor,
                   (__bridge id)[UIColor colorWithRGBHex:0x00c7b7].CGColor];
    }
    else if (anglePercent>1&&anglePercent<=2) {//第2圈
        colors0 = @[(__bridge id)[UIColor colorWithRGBHex:0x00c7b7].CGColor,
                   (__bridge id)[UIColor colorWithRGBHex:0x4e75af].CGColor];
    }else if(anglePercent>2&&anglePercent<=3){//第3圈
        colors0 = @[(__bridge id)[UIColor colorWithRGBHex:0x684cb6].CGColor,
                   (__bridge id)[UIColor colorWithRGBHex:0xa1377a].CGColor];
    }
    
    //右半圆
    NSArray * colors1 = [NSArray array];
    if (anglePercent <= 0.01) {//第0圈
        colors1 = @[(__bridge id)[UIColor colorWithRGBHex:0xf9f9f9].CGColor,
                    (__bridge id)[UIColor colorWithRGBHex:0xf9f9f9].CGColor];
    }
    else if (anglePercent>0.01&&anglePercent<=1) {//第1圈
        colors1 = @[(__bridge id)[UIColor colorWithRGBHex:0x00c7b7].CGColor,
                    (__bridge id)[UIColor colorWithRGBHex:0x00c7b7].CGColor];
    }
    else if (anglePercent>1&&anglePercent<=2) {//第2圈
        colors1 = @[(__bridge id)[UIColor colorWithRGBHex:0x4e75af].CGColor,
                    (__bridge id)[UIColor colorWithRGBHex:0x684cb6].CGColor];
    }else if(anglePercent>2&&anglePercent<=3){//第3圈
        colors1 = @[(__bridge id)[UIColor colorWithRGBHex:0xa1377a].CGColor,
                    (__bridge id)[UIColor colorWithRGBHex:0xde1c40].CGColor];
    }


    CALayer *gradientLayer = [CALayer layer];
    
    CAGradientLayer *gradientLayer1 =  [CAGradientLayer layer];
//    [gradientLayer1 setLocations:@[@0.1,@0.5,@1]];
    gradientLayer1.frame = CGRectMake(self.frame.size.width/2, 0, self.frame.size.width/2, self.frame.size.height);
    [gradientLayer1 setColors:colors0];
    [gradientLayer1 setStartPoint:CGPointMake(0.5, 0)];
    [gradientLayer1 setEndPoint:CGPointMake(0.5, 1)];
    [gradientLayer addSublayer:gradientLayer1];
    
    CAGradientLayer *gradientLayer2 =  [CAGradientLayer layer];
//    [gradientLayer2 setLocations:@[@0.5,@0.9,@1 ]];
    gradientLayer2.frame = CGRectMake(0, 0, self.frame.size.width/2, self.frame.size.height);
    [gradientLayer2 setColors:colors1];
    [gradientLayer2 setStartPoint:CGPointMake(0.5, 1)];
    [gradientLayer2 setEndPoint:CGPointMake(0.5, 0)];
    [gradientLayer addSublayer:gradientLayer2];
    
    
    [self.layer addSublayer:gradientLayer];
   
}
```
***

- created，150719