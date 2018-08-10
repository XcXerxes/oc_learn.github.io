---
description: 不可变数值。OC中的集合不再是单纯的数组，而分为了三大类：数组，字典，集。
---

# NSArray

学习数组，首先明白几个内容：

* 数组分为不可变数组\(NSArray\)与可变对象\(NSMutableArray\)；
* 只能装对象，基本数据类型包装成NSNumber，复杂类型包装成 NSValue；
* nil 作为结束标示；
* 下标从 0 开始；
* 数组中的元素不一定为一个类型， 可以存在 NSString，也可以存在 NSNumber。

#### 常用的方法

与不可变字符串 NSString 类似，不可变数组 NSArray 操作也不在原数组上进行，而是通过返回一个新的数组来操作。

* [初始化](nsarray.md#chu-shi-hua)
* [获取数组元素个数](nsarray.md#huo-qu-shu-zu-yuan-su-ge-shu)
* [添加元素](nsarray.md#tian-jia-yuan-su)
* [查找元素](nsarray.md#cha-zhao-yuan-su)
* [获取元素](nsarray.md#huo-qu-yuan-su)
* [截取部分元素](nsarray.md#jie-qu-bu-fen-yuan-su)
* [数组排序](nsarray.md#shu-zu-pai-xu)

#### 初始化

简化操作

```objectivec
NSArray *array1 = @[@"1", @"2", @"3"];
```

实例方法

```objectivec
// 按元素初始化(常用)
NSAaary *array2 = [[NSArray alloc] initWithObjects:@"1", @"2", @"3", nil];

// 按已有的数组进行初始化(初始化出来和array1的元素相同)
NSArray *array3 = [[NSArray alloc] initWithArray:array1];
```

遍历构造

```objectivec
NSArray *array4 = [NSArray arrayWithObjects:@"1", @"2", @"3", nil];
NSArray *array5 = [NSArray arrayWithArray:array1];
```

#### 获取数组元素个数

```objectivec
NSUInteger count1 = [array1 count]; // 3
NSUInteger count2 = array1.count; // 3
```

#### 添加元素

```objectivec
// 从尾部追加一个元素
NSArray *array6 = [array1 arrayByAddingObject:@"4"];

// 从尾部依次追加 array2 中的元素
NSArray *array7 = [array1 arrayByAddingObjectsFromArray:array2];
```

#### 查找元素

是否包含某元素

```objectivec
BOOL isContain = [array2 constainsObject:@"1"];
```

元素具体位置

查找并返回元素的具体位置，如果没找到，返回NSNotFound。

```objectivec
NSUInteger index = [array2 indexOfObject:@"2"];
NSLog(@"是否找到=== %d", index == NSNotFound);
```

#### 获取元素

存入的是什么类型，取出的时候，就需要什么类型去接收。

根据下标获取元素

```objectivec
// 两种方式等价
NSString *string1 = array2[0];
NSString *string2 = [array2 objectAtIndex:0];

// 取出第一个和最后一个元素
NSString *string3 = [array2 firstObject]; // 第一个元素
NSString *string4 = [array2 lastObject]; // 最后一个元素
```

#### 截取部分元素

通过给定范围，截取数组元素中的部分元素，存入新数组

```objectivec
NSArray *array8 = [array2 subarrayWithRange:NSMakeRange(0, 2)];
```

#### 数组排序

一般的字符串数组排序会使用到 NSString 提供的 compare: 或 caseInsensitiveCompare 方法，然后调用 sortedArrayUsingSelector 达到对 array2 中，存储的字符串进行排序的目的。

```objectivec
NSArray *array9 = [array2 sortedArrayUsingSelector:@selector(caseInsensitiveCompare:)];
```

注意上面写法的几点要求：

* array2 数组中存储的全是字符串；
* 比较大小需要传的 `@selector` 类型中，`caseInsensitiveCompare` 是字符串中的方法；
* 这里选择器 `@selector` 返回值类型必须是 `NSComparisonResult`；

