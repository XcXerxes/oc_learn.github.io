---
description: self 和 super 是OC 中最常用的两个关键字，它们常用在对象初始化方法里。
---

# self 和 Super

首先看看OC 中 对象的初始化方法：

```objectivec
@implementation Son : Father

- (id) init {
  self = [super init];
  if (self) {
  }
  return self;
}
```

相信很多人对这段代码很熟悉，在读完之后你需要 明白几个问题：

* self 和 super 是什么
* \[super init\] 做了什么
* 为什么要把 \[super init\] 赋值给self？

#### 问题1解析：

self 是一个隐藏参数变量，指向当前调用方法的对象，还有一个隐藏参数是 _cmd，代表当前方法 selector，在runtime 时会_调用 objc\_msgSend\(\) 方法。

super 并不是隐藏参数，只是编译器的指令符号，在 runtime 时调用 objc\_msgSendSuper\(\) 方法。

官方文档针对 self 和 super 有这样的解释

{% hint style="info" %}
**官方文档中self相关解释**  
 Whenever you’re writing a method implementation, you have access to an important hidden value, self. Conceptually, self is a way to refer to “the object that’s received this message.” It’s apointer, just like the greeting value above, and can be used to call a method on the current receiving object.  
 **super解释**  
 There’s anotherimportant keyword available to you in Objective-C, called super. Sending a message to super is a way to call through to a method implementation defined by a superclass further up the inheritance chain. The most common use of super is when overriding a method.
{% endhint %}

大概的理解是：

{% hint style="info" %}
self 调用自己方法，super 调用父类的方法

self 是类，super 是预编译指令

\[self class\] 和 \[super class\] 输出是一样的
{% endhint %}

#### 问题2解析:

想要明白这个问题需要理解 self 和 super 的底层实现原理。

self 调用方法：

* 当使用 self 调用方法时，会首先从当前类的方法列表中开始寻找，如果没有再从父类中寻找；在 runtime 时会使用 objc\_msgSend\(\) 函数：

```objectivec
id objc_msgSend(id theReceiver, SEL theSelector, ...)
```

第一个参数是消息接收者，第二个参数是调用的具体类方法的 selector，后面是 selector 方法的可变参数。

以 \[self setSize:\] 为例，编译器会转换成 objc\_msgSend 的函数调用，其中 theReceiver 是 self,  theSelector 是 @selector\(setSize:\)。这个 selector 是从当前 self 的 class 的方法列表开始找的 setSize，一旦找到后把对应的 selector 传递过去。

Super 调用方法：

* 当使用 super 时，则从父类的方法列表中开始找，然后调用父类的这个方法。在 runtime 时会调用 objc\_msgSendSuper 函数：

```objectivec
id objc_msgSendSuper(struct objc_super *super, SEL op, ...)
```

第一个参数时 objc\_super 的结构体， 第二个参数时类似 objc\_msgSend 函数的 selector。 而 objc\_super 的结构体如下：

```objectivec
struct objc_super {
    id receiver;
    Class superClass
};
```

* 当编译器遇到 \[super setSize\] 时， 会做几件事：

1、构建 objc\_super 的结构体，此时这个结构体的第一个成员变量 receiver 就是子类， 和 self 中相同。而第二个成员变量 superClass 就是指父类， 调用 objc\_msgSendSuper 方法，将这个结构体和 setSize 的 selector 传递出去。

2、函数里面做的事情类似这样：从 objc\_super 结构体指向的 superClass 的方法列表开始找 setSize 的selector， 找到后再用 objc\_super -&gt; receiver 去调用这个 selector。

这就是为啥 \[self class\] 和 \[super class\] 输出结果会是一样的。

#### 问题3解析：

OC 具有类继承性， 子类继承父类从而获得父类相关的属性和方法，所以再子类的初始化方法中，必须首先调用父类的初始化方法，完成父类相关资源的初始化。

\[super init\] 去 self  的 super 中调用 init，然后 super 会调用其父类的 init，以此类推，直到找到根类 NSObject 的 init ，然后根类中的 init 负责初始化内存区域，添加一些必要的属性，返回内存指针，延着继承链，指针从上到下进行传递，同时在不同的子类中可以向内存添加必要的属性。最后直到我们当前类中把内存地址赋值给 self 参数。如果调用 \[super init\] 失败的话， 通过 判断 self 来决定是否执行子类的初始化操作。

下面两个例子来理解 self 和 super:

```objectivec
@implementation Son : Father

- (id) init {
    self = [super init];
    if (self) {
        NSLog(@"%@", NSStringFormClass([self class]));
        NSLog(@"%@", NSStringFormClass([super class]));
    }
    return self;
}
@end
```

控制台输出结果：

```text
Son
Son
```

当发送 class 消息时不管是用 self 还是用 super， 接收消息主体依然是 self ，也就是说 self 和 super 指向同一个对象，所以都会打印出Son。

```objectivec
@interface Father : NSObject
- (void)printCurrentClass;
@end

@implementation Father

- (void)printCurrentClass {
    NSLog(@"printCurrentClass:%@", [self class]);
}
@end

@interface Son: Father
- (void)printSuperClass;
@end

@implementation Son

- (void)printSuperClass {
    [super printCurrentClass];
}
@end

// 调用方法
Son *son = [Son new];
[son printCurrentClass]; // 直接调用父类方法， 子类没有重载
[son printSuperClass]; // 调用子类的方法
```

控制台输出结果：

```text
printCurrentClass: Son
printCurrentClass: Son
```

printCurrentClass 方法体中 self 始终指向方法的接收者对象 Son，倘若换成 \[super class\]，结果都是一样。

