# 布尔类型

在 OC 中， 布尔类型的关键字为 大写的 BOOL，表示方式为 YES，NO。

#### 使用方式：

BOOL 类型一般用于 表示真假、是否这种情况，如 ios 开发中的按钮是否选中，排序是否提前完成等。表示方式如下：

```text
BOOL isSelected = NO;

// 判断方式
if (isSelected) {
    NSLog(@"选中...")
}
```

#### 深入理解

当在 xcode 中输入 BOOL 时， 编译器还联想出了 bool，Boolean，boolean\_t 等类型， 我们一一来分析一下。

* BOOL

头文件：objc.h

实际类型：signed char

表示方式：YES / NO

注意点：当 BOOL a == 1 的时候， 才是YES，其他情况均为 NO，所以在使用时，慎用 a == YES 这种方式。

```objectivec
BOOL a = 3;
NSLog(@"%d", a == YES); // 输出0
```

* bool

头文件：stdbool.h

实际类型：\_Bool \(int\)

表示方式：true / false

```objectivec
bool a = 3;
NSLog(@"%d", a == true); // 输出1
```

* Boolean

头文件：MacTypes.h

实际类型：unsigned char

表示方式：TRUE / FALSE

注意：与BOOL的情况相同，  仅当 a == 1 时为 TRUE

```objectivec
Boolean a = 1;
NSLog(@"%d", a == TRUE);
```

* boolean\_t

头文件：boolean.h

实际类型：unsigned int

关于在 OC 中几种布尔值的区别，可查看这篇在 stackoverflow 的回答：

{% hint style="info" %}
[http://stackoverflow.com/questions/14464671/all-kinds-of-booleans-in-objectivec](http://stackoverflow.com/questions/14464671/all-kinds-of-booleans-in-objectivec)
{% endhint %}

