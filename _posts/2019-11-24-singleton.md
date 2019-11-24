---
layout: post
title: 非主流单例
description: 非主流单例
categories: 学习总结
keywords: 单例
---

## 前言

单例模式大家其实都已经熟悉了，写个单例也是轻车熟路。

```objc
@interface Manager: NSObject

- (instancetype)init NS_UNAVAILABLE;
+ (instancetype)new NS_UNAVAILABLE;
+ (instancetype)sharedInstance;

@end

@implementation Manager

+ (instancetype)sharedInstance {
    static id _sharedInstance = nil;
    static dispatch_once_t oncePredicate;
    dispatch_once(&oncePredicate, ^{
        _sharedInstance = [[self alloc] init];
    });
    return _sharedInstance;
}

@end
```

以上是一个简单的单例，如果要写一个完整的单例，还要重写 `allocWithZone:` `copyWithZone:` `retain` `release` 等方法。

但是在我们自己的内部使用时，以上写法即可。只是需要注意，使用时只调用 `sharedInstance` 方法, 不要调用 `alloc` `copy` 等方法。

这些不是我们要讨论的重点，今天我们要讨论的是另外两种非主流的形式。

## 1.弱引用单例

弱引用单例跟普通的单例类似，只是有一点区别：没有引用时会被销毁内存。

我们知道，普通的单例在程序运行的整个生命周期中，只有一份内存，也就是引用计数始终为 1。一旦该对象被创建，在程序运行期间就不会被销毁，直至程序结束。而弱引用单例，当有引用时，其内存只有一份，当没有引用时，就被释放。

实现也比较简单：

```objc
@interface Manager : NSObject

- (instancetype)init NS_UNAVAILABLE;
+ (instancetype)new NS_UNAVAILABLE;
+ (instancetype)sharedManager;

@end

@implementation Manager

- (void)dealloc {
  NSLog(@"Manager 释放了");
}

+ (instancetype)sharedManager {
  static __weak Manager *weakInstance = nil;
  Manager *sharedInstance = weakInstance;
  @synchronized (self) {
    if (sharedInstance == nil) {
      sharedInstance = [[Manager alloc] init];
      weakInstance = sharedInstance;
    }
  }
  return sharedInstance;
}

@end
```

我们先声明了一个全局的 weak 指针，每次调用 `sharedManager` 时，都会先取 weak 指针指向的对象，如果取不到，就创建新对象，并将其赋值给 weak 指针。这样，只要该对象存在，每次返回的都是同一个对象，如果没有外界引用，weak 指针就会指向 nil。

## 2.类对象单例

单例模式想达到的效果是：全局只有一个对象，以保证该对象可以在不同模块共享。而 Objective-C 本身其实自带这种效果，这就是`类对象`。我们知道，Objective-C 中，对象可以分为三种，即`实例对象`、`类对象`、`元类对象`。元类对象我们平时开发不会用到，但类对象其实我们是经常用到的。而类对象在内存中只有一份，既然如此，我们何不用类对象来实现单例模式呢！

```objc
// Manager.h

@interface Manager : NSObject

@property (class, nonatomic, strong) NSString *name;

+ (void)hello;

@end

// Manager.m

static NSString *_name = nil;

@implementation Manager

+ (void)setName:(NSString *)name {
  _name = name;
}

+ (NSString *)name {
  return _name;
}

+ (void)hello {
  NSLog(@"Hello, %@!", self.name);
}

@end
```

使用起来就是这样的：

```objc
int main(int argc, char * argv[]) {
  Manager.name = @"Lily";
  [Manager hello]; // Hello, Lily!
  return 0;
}
```

看来，一个小小的单例，也有许多玩法。:)





