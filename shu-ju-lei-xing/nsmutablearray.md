---
description: >-
  可变数组，顾名思义即可以对数组内的元素数量进行增加，删除，同时也可以对存储元素的内容进行修改。由于 NSMutableArray 类是 NSArray
  的子类，因此 NSArray 类的方法和属性，NSMutableArray 类都可以继承使用。
---

# NSMutableArray

#### 可变数组创建

可变数组在初始化时可以设置容量为任意值，在执行一些列插入删除等操作时数组会根据元素的数量自动改变容量大小。通常情况，可变数组可以使用以下几种方式来初始化：

* 使用 array 方法，使用该方法时，不指定可变数组的容量，使用起来比较简单，也是常用方法。
* 使用 arrayWithCapacity 方法，类方法，需要提供数组的初始容量；
* 使用 initWithCapacity 方法，实例方法，需要提供数组的初始容量；

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 初始化
        NSMutableArray *mArray1 = [NSMutableArray array];
        NSMutableArray *mArray2 = [NSMutableArray arrayWithCapacity: 100];
        NSMutableArray *mArray3 = [[NSMutableArray alloc] initWithCapacity: 100]
    }
    return 0;
}
```

#### 可变数组的元素操作

与不可变数组相比，可变数组可以对其中的元素进行修改操作，常用的有如下几个：

* 在数组末尾增加某个元素

```objectivec
- (void)addObject:(ObjectType)anObject;
```

* 在指定位置增加元素

```objectivec
- (void)insertObject:(ObjectType)anObject atIndex:(NSUInteger)index;
```

* 替换某个下标的元素

```objectivec
- (void)replaceObjectAtIndex:(NSUInteger)index withObject:(ObjectType)anObject;
```

* 删除元素

```objectivec
- (void)removeObject:(ObjectType)anObject;
```

* 删除指定下标的元素

```objectivec
- (void)removeObjectAtIndex:(NSUInteger)index;
```

* 删除所有元素

```objectivec
- (void)removeAllObject;
```

* 修改某个元素对象的值。我们可以使用赋值运算符直接更新数组中某个下标对象的值。

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 初始化
        NSMutableArray *mArray = [NSMutableArray arrayWithObjects:@"leo", @"Antony", nil];
        NSLog(@"初始状态，数组中第一个对象的值：%@", mArray[0]); // leo
        mArray[0] = @"www.baidu.com";
        NSLog(@"更新后，数组中第一个对象的值：%@", mArray[0]);  // www.baidu.com
    }
    return 0;
}
```



