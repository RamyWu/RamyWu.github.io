---
layout: post
title:  "iOS开发笔记-初学GCD"
date:   2015-05-28 21:40:00 +0800
tags: iOS
category: Work
---

~iOS笔记：GCD（Grand Central Dispatch）

## 1.Dispatch Queues

### 1.1 GCD中有三种队列类型：

1. **主队列**：`dispatch_get_main_queue()`；与主线程功能相同；串行队列，做线程间通讯的（后台线程完成工作后，通知主线程更新UI）；
2. **全局队列（并发队列）**： `dispatch_get_global_queue`函数，第一个参数传入优先级，第二个参数传0；并发队列，能够开多条线程；
3. **用户队列（串行队列）**： `dispatch_queue_create`创建；第一个参数是一个标签，这纯是为了debug，第二个参数目前还不支持，传入NULL就行；串行队列，最多能开一条线程；

### 1.2 提交任务

1. **同步任务**：`dispatch_sync`，传入队列和代码块；顺序执行，不会开启新线程

2. **异步任务**：`dispatch_async`，传入队列和代码块；要在别的线程并发执行，会开启线程；


### 1.3 队列和任务的组合

**主队列**

* 同步任务 ：不开线程，会造成**死锁**！
* 异步任务 ：不开新线程，顺序执行

**全局队列（并发队列）**

* 同步任务：不开启新线程，任务顺序执行
* 异步任务：会开启多条线程，任务不是顺序执行


**用户队列（串行队列）**

* 异步任务：只会开启一条线程，所有任务顺序执行
* 同步任务：不会开启新线程，所有任务顺序执行


###1.4 常用的GCD代码示例：

**全局队列的异步任务+主线程更新界面：**

```
dispatch_async(dispatch_get_global_queue(0, 0), ^{
    //需要在后台执行的代码块;
    ...
    ...
    
    //执行完毕在主线程更新UI
    dispatch_async(dispatch_get_main_queue(), ^{
		//更新UI操作
		...
		...
    });
});
      
```
**同步任务（先执行完同步再进行异步任务）+ 异步任务进行网络操作**

要用于同步机制，queue必须是一个用户队列，而非全局队列


```

    dispatch_queue_t q = dispatch_queue_create("myQueue", DISPATCH_QUEUE_CONCURRENT);
	 
	 //异步任务进行网络操作        
    dispatch_async(q, ^{
        // 同步任务
        dispatch_sync(q, ^{
            ...
        });
        
        //异步任务
        for (int i = 0 ; i<count ; i++) {
            dispatch_async(q, ^{
            	...
            });
        }

    });


```
***

## 2.Dispatch Group


* 当异步任务完成后，我们如果需要对任务结果进行操作。

* 将多个任务组成一组，监测这些任务全部完成或者等待全部完成时发出消息通知用户。

* 使用`dispatch_group_create()`创建；

* 使用`dispatch_group_enter`和`dispatch_group_leave`搭配使用，可以讲多个异步任务入组并监听和通知;并且dispatch_group_leave 要放在 任务block中的最后一句。

### 示例代码

```
	// 创建组
    dispatch_group_t group = dispatch_group_create();
    
    dispatch_queue_t q = dispatch_get_global_queue(0, 0);

    	// 将任务入组
    dispatch_group_enter(group);
    
    for (int i = 0; i<count; i++) {
        dispatch_group_async(group, q, ^{
            ...
            ...
            //任务完成离开
            dispatch_group_leave(group);
        });
    }
    
    // 接收通知，通知用户
    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
    	//操作结果
    	...
    	...
    });

```
***

## 3.Dispatch Once

**单例**，`dispatch_once`在多线程执行时能够保证只执行一次block块代码。

```
  // static保存在静态区，程序启动就存在，随着程序一起销毁
  static dispatch_once_t onceToken;  
  dispatch_once(&onceToken, ^{
      ...
      ...
  });  
    
```

***

## 4.Dispatch After

**延时处理**:调用`dispatch_after`函数；


```

//DISPATCH_TIME_NOW 当前时间
//(int64_t)(2.0 * NSEC_PER_SEC)) 延迟时间2.0秒

dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        ...
        ...
    });

```
***

## 5.Dispatch Sources


未完待续

---

- created，150528