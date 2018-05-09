---
layout: post
title:  "iOS开发笔记-自定义大头针"
date:   2015-06-08 18:45:00 +0800
tags: iOS
category: Work
---

~iOS笔记

### 项目背景

不说废话~首先，项目的目的是：

* 了解地图中的**大头针**是什么，有什么样的作用，如果自定义，所需要的条件？
* 在使用地图开发过程中，所用到了哪些方法和代理方法，如何使用我们自定义的大头针；
* 在自定义过程中仍不能忘记封装，养成MVC模式的好习惯

***

### 项目结构分层

##### 控制器 -> 自定义annoView  -> 自定义annotation模型 -> 数据模型myTuangou

第一层（最外层）： 视图控制器

* 拥有模型数组，每一个元素存放服务器返回的数据模型MyTuangou
* 拥有mapView，实现代理方法，展示大头针View以及点击大头针View时做的操作；

第二层：自定义AnnotationView

* 提供控制器的快速创建的类方法，类似cell的创建；
* 重写setAnnotaion方法将自定义anno模型的图片赋值进来；

第三层：自定义MyAnnotation模型

* 数据模型：MyTuangou * tuangou;
* 位置： CLLocationCoordinate2D coordinate;
* 是否展示描述：BOOL showDesc;

第四层（最底层）：服务器返回的真正数据模型：MyTuangou

* 标题，描述，图片，描述图片，显示位等属性（根据需求，展示数据）


（PS：如果点击大头针显示自定义的详细描述的框，则实现原理是在当前位置的mapView上再添加一个详细描述MyDescAnnotationView的大头针!
内部分层和上述类似，但是descAnnotationView添加多了一层descView，里面也有一个myTuangou的数据模型）

***


### 主要代码

#### 第一个大头针

##### 控制器

1.添加大头针
		
```
/**
 *  添加大头针
 */
- (IBAction)addAnnotation {
    for (MyTuangou * tg in self.arrTuangous) {
        MyTuangouAnnotation * tgAnno = [[MyTuangouAnnotation alloc] init];
        tgAnno.tuangou = tg;
        [self.mapView addAnnotation:tgAnno];
    }
}
```

2.遵循MKMapViewDelegate协议，MKMapViewDelegate方法

显示大头针

```
/**
 *  大头针的view
 */
-(MKAnnotationView *)mapView:(MKMapView *)mapView viewForAnnotation:(id<MKAnnotation>)annotation
{
    if ([annotation isKindOfClass:[MyTuangouAnnotation class]]) {
        MyTuangouAnnotationView * annoView = [MyTuangouAnnotationView annotationViewWithMapView:mapView];
        
        //传递模型
        annoView.annotation = annotation;
        return annoView;
    }else if([annotation isKindOfClass:[MyTuangouDescAnnotation class]]){
        MyTuangouDescAnnotationView * descAnnoView = [MyTuangouDescAnnotationView descAnnotationViewWithMapView:mapView];

        descAnnoView.annotation = annotation;
        return descAnnoView;
    }
    return nil;
}
```

3.点击大头针

```
/**
 *  点击选中一颗大头针
 */
-(void)mapView:(MKMapView *)mapView didSelectAnnotationView:(MKAnnotationView *)view{
    if ([view isKindOfClass:[MyTuangouAnnotationView class]]) {
        MyTuangouAnnotation * anno = view.annotation;

        //如果显示了详细描述，就返回
        if (anno.showDesc)return;

        //1.删除以前的MyTuangouDescAnnotation
        for (id annotation in mapView.annotations) {
            if ([annotation isKindOfClass:[MyTuangouAnnotation class]]) {
                //如果不是descAnno，就把showDesc = no
                MyTuangouAnnotation * tgAnno = annotation;
                tgAnno.showDesc = NO;
                
            }else if ([annotation isKindOfClass:[MyTuangouDescAnnotation class]]){//如果是descAnno,就移除
                [mapView removeAnnotation:annotation];
            }
        }
        //2.再添加最新的desc大头针
        MyTuangouDescAnnotation * descAnno = [[MyTuangouDescAnnotation alloc] init];
        descAnno.tuangou = anno.tuangou;
        [mapView addAnnotation:descAnno];
        
        
    }else if ([view isKindOfClass:[MyTuangouDescAnnotationView class]]){
    //跳转控制器等代码

    }
}

```

***

##### 自定义AnnotationView

快速创建

```
+(instancetype)annotationViewWithMapView:(MKMapView *)mapView{

    static NSString * annoID = @"tuangou";
    MyTuangouAnnotationView * annoView = (MyTuangouAnnotationView *)[mapView dequeueReusableAnnotationViewWithIdentifier:annoID];
    if (!annoView) {
        annoView = [[MyTuangouAnnotationView alloc] initWithAnnotation:nil reuseIdentifier:annoID];
    }
    return annoView;
}
```

***

##### 自定义MyAnnotation模型

属性

```
/**
 *  团购数据模型
 */
@property(strong,nonatomic)MyTuangou * tuangou;

/**
 *  位置
 */
@property (assign,nonatomic) CLLocationCoordinate2D coordinate;

/**
 *  是否展示描述
 */
@property (nonatomic, assign, getter = isShowDesc) BOOL showDesc;

```
	
重写set方法 

```
-(void)setTuangou:(MyTuangou *)tuangou{
    _tuangou = tuangou;
    self.coordinate = tuangou.coordinate;
}
```


***

##### 服务器返回的MyTuangou数据模型
		
		略


***

#### 详细描述

##### 自定义DescAnnotationView

快速创建

```
+(instancetype)descAnnotationViewWithMapView:(MKMapView *)mapView{
    static NSString * descAnnoID = @"tuangouDescAnno";
    MyTuangouDescAnnotationView * descAnnoView = (MyTuangouDescAnnotationView *)[mapView dequeueReusableAnnotationViewWithIdentifier:descAnnoID];
    if (descAnnoView == nil) {
        descAnnoView = [[MyTuangouDescAnnotationView alloc] initWithAnnotation:nil reuseIdentifier:descAnnoID];
    }
    return descAnnoView;
}
```
	
初始化，将descView添加到子视图
		
```
-(instancetype)initWithFrame:(CGRect)frame{
     self = [super initWithFrame:frame];
    if (self) {
        self.backgroundColor = [UIColor clearColor];
        MyTuangouDescView* descView = [MyTuangouDescView tuangouDescView];
        descView.y = 0;
        [self addSubview:descView];
        self.descView = descView;

        self.frame = CGRectMake(0, 0, descView.frame.size.width, descView.frame.size.height + 80);
        
    }
    return self;
}

```
重写set方法
	
```
-(void)setAnnotation:(id<MKAnnotation>)annotation{
    [super setAnnotation:annotation];
    
    MyTuangouDescAnnotation * anno = annotation;
    self.descView.tuangou = anno.tuangou;
}
```
	
当一个控件被添加到父控件的时候调用

```
/**
 *  UIView动画可以多次显示，如果是图层动画只能显示一次，这个方法实现动画暂时是有问题的
 */
-(void)didMoveToSuperview{
    CAKeyframeAnimation * anim = [CAKeyframeAnimation animation];
    anim.keyPath = @"transform.scale";
    anim.duration = 0.5;
    anim.values = @[@0, @1.5, @1, @1.5, @1];

    [self.layer addAnimation:anim forKey:nil];
    // UIView动画
    //    self.alpha = 0;
    //
    //    [UIView animateWithDuration:2 animations:^{
    //        self.alpha = 1;
    //    }];

}

```

***

##### 展示描述界面的MyTuangouDescView，是用xib描述

属性：Tuangou模型，还有连接的插座变量

```
@property (weak, nonatomic) IBOutlet UIImageView *imageView;
@property (weak, nonatomic) IBOutlet UILabel *lblTitle;
@property (weak, nonatomic) IBOutlet UILabel *lblSubtitle;

@property(strong,nonatomic)MyTuangou * tuangou;

```

快速创建

```
+(instancetype)tuangouDescView{
    return [[[NSBundle mainBundle] loadNibNamed:@"MyTuangouDescView" owner:nil options:nil] firstObject];
}
```

重写set方法

```
-(void)setTuangou:(MyTuangou *)tuangou{
    _tuangou = tuangou;
    
    self.lblTitle.text = tuangou.title;
    self.lblSubtitle.text = tuangou.desc;
    self.imageView.image = [UIImage imageNamed:tuangou.image];
    
}
```

***

##### 自定义MyDescAnnotation模型

	和myAnnotation模型类似

***


- created，150608