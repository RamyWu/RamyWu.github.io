---
layout: post
title:  "iOS开发笔记-图片异步缓存"
date:   2015-05-21 23:25:00 +0800
tags: iOS
category: Work
---

~iOS笔记

由于初学GCD，运用不熟练，这次开发加强了GCD的运用以及多线程的学习中对同步和异步任务和理解。下一篇是学习多线程的笔记（GCD的简单运用）。


## 项目背景


开发需求：从服务器拉取一堆人脸图片Url，根据图片进行人脸检测和裁剪，将裁剪出来的人脸图片显示在cell.imageView中。

### 原始方案：

在服务器获取图片Url完毕的回调中做裁剪并缓存，缺点：一次性加载所有图片，并且第一次加载会卡（因为裁剪人脸会耗时），不推荐，应该是显示时再加载

1. 在从服务器拉取数据时使用SDWebImage框架异步缓存每张原始图片；

2. 缓存完毕回调中，对每张图进行一次人脸检测，并将裁剪的人脸图片保存在一个字典dicImg里，如果需要也将原图也保存进来。
 
3. 创建NSMutableDictionary的property属性：dicDatas，将每张图对应的字典添加进来，每个dicImg的key对应是图片的URL；

4. 将dicDatas字典缓存到沙盒tmp目录下，下次再取图片就不用从服务器拉取而是从缓存读取。

### 原始方案代码：

**图片获取**

```

	 //从沙盒tmp目录下找缓存文件
    self.dicDatas = [NSMutableDictionary dictionaryWithContentsOfFile:kTmpPath];

    //如果没又找到，则从服务器拉取数据
    
    if (!self.dicDatas) {
        self.dicDatas = [NSMutableDictionary dictionary];
        [self reloadAllData];
    }
    
```

**拉取数据**

```

-(void)reloadAllData{
            
        __weak typeof(self) weakSelf = self;
        
        //1.dispatch group：用来将多个任务组成一组，可以监测这些异步任务全部完成或者等待全部完成时发出的消息
        
        dispatch_group_t g = dispatch_group_create();
        
        //2.从服务器获得的URL保存在self.arrUrls数组里
        for (NSString * strUrl in self.arrUrls) {
            
            NSString *str = [strUrl stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
            NSURL * url = [NSURL URLWithString:str];
            
        	  //3.将异步任务们任务添加到组里
            dispatch_group_enter(g);
            
            //4. 缓存图片
            [[SDWebImageManager sharedManager] downloadImageWithURL:url options:0 progress:nil completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType, BOOL finished, NSURL *imageURL) {

            		//4. 人脸检测并裁剪图片
                UIImage * faceImage = [CutTool cutFaceImage:image];
                
                NSMutableDictionary * dicImgs = [NSMutableDictionary dictionary];

                NSData *dataOrigin = UIImageJPEGRepresentation(image,1.0);
                NSData *dataFace = UIImageJPEGRepresentation(faceImage,1.0);
                
            		//5. 将图片转化为data保存在字典里

                [dicImgs setObject:dataOrigin forKey:@"originImage"];
                [dicImgs setObject:dataFace forKey:@"faceImage"];
                
                
            		//6. 将每张图片对应字典插入属性字典里

                [weakSelf.dicDatas setObject: dicImgs forKey:[imageURL absoluteString]];

                //7.完成一个任务，离开组
                dispatch_group_leave(g);
            }];
            
        }
        //8.通知提醒，更新UI
        
        dispatch_group_notify(g, dispatch_get_main_queue(), ^{
            if (weakSelf.dicDatas.count) {
            	//9. 将数据缓存在沙盒tmp中

                BOOL isSave = [weakSelf.dicDatas writeToFile:kTmpPath atomically:YES];
                NSLog(@"isSave ==== %d",isSave);

                [self.table reloadData];
            }
        });
}

```

**cell显示图片**

```

NSDictionary * dic = [self.dicDatas objectForKey:url];
    if (dic) {
        NSData * dataImage = [dic objectForKey:@"faceImage"];
        cell.imageView.image = [UIImage imageWithData:dataImage];
    }else{
        cell.imageView.image = [UIImage imageNamed:@"faceDefault"];
    }

```

***


### 最佳方案：需要显示图片时才将裁剪并缓存

1. cellForRowAtIndexPath方法中，使用SDWebImage框架的分类方法缓存图片；

2. 然后在缓存completed回调中裁剪人脸工具类的方法，将返回的人脸图片显示在cell.imageView中。

3. 裁剪人脸工具类方法：人脸图片先从内存取，如果内存中没有再去缓存取，如果什么都没有就将原图片裁剪人脸并缓存便于下次使用。

### 最佳解决方案代码：

外部：

```
[cell.imageView sd_setImageWithURL:url placeholderImage:[UIImage imageName:@"facePlaceholder"] completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType, NSURL *imageURL) {
	
	//人脸工具类方法将裁剪好的人脸图片重新显示
	cell.imageView.image = [CutFaceTool faceImageWithURL:imageURL image:image];
	
}];

```

内部：

```
-(UIImage *)faceImageWithURL:(NSURL *)imageURL image:(UIImage *)image
{
	//拼接URL
    NSURL *faceImageURL = [NSURL URLWithString:[@"face:" stringByAppendingString:imageURL.absoluteString]];
    
    UIImage * faceImage = nil;
    
    //检测缓存里是否有此人脸图片
    if (![[SDWebImageManager sharedManager] diskImageExistsForURL:faceImageURL]) 
    {
        faceImage = [[FSCutFaceImageTool cutFaceImageTool] cutFaceWithImage:image];
        [[SDWebImageManager sharedManager] saveImageToCache:faceImage forURL:faceImageURL];
  
    }else{
    
        NSString *cacheKey = [[SDWebImageManager sharedManager] cacheKeyForURL:faceImageURL];
        faceImage = [[[SDWebImageManager sharedManager] imageCache] imageFromMemoryCacheForKey:cacheKey];
        if (faceImage == nil) {
            faceImage = [[[SDWebImageManager sharedManager] imageCache] imageFromDiskCacheForKey:cacheKey];
        }
    }
    return faceImage;
}

```

~ 后来leader又不断优化了这部分，自己写的还是渣...

***

- created，150521