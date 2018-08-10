---
description: >-
  可变字典是不可变字典的子类。NSMutableDictionary 类继承了 NSDictionary
  类的属性和方法之外，与不可变字典相比，其存储的键值可以新增，删除以及修改
---

# NSMutableDictionary

#### 实例化可变字典对象

在开发中， 当我们需要实例化一个可变字典对象时，可以使用如下的一些方法。

* 快速简易方法：使用 dictionary 方法来初始化一个空的可变字典对象，

```objectivec
+ (instancetype)dictionary;
```

* dictionaryWIthCapacity 方法：使用该方法创建可变字典对象， 需要预先指定可变字典的容量。

```objectivec
+ (instancetype)dictionaryWithCapacity:(NSUInteger)numItems;
```

* initWithContentsOfFile 方法：从指定文件初始化得到一个可变字典对象。

```objectivec
- (nullable NSMutableDictionary<KeyType, ObjectType> *)initWithContentsOfFile:(NSString *)path;
```

#### 增加键值对

与不可变字典相比，通过调用 setObject:forKey 方法，可以增加可变字典的键值

```objectivec
- (void)setObject:(ObjectType)anObject forKey:(KeyType <NSCopying>)aKey;
```

下方的代码中，首先创建了一个空的可变字典，然后再该字典中插入了两个键值对，最后打印出所有的键值对

```objectivec
int main(int argc, const char *argv[]) {
    @autoreleasepool {
        // 实例化一个可变字典
        NSMutableDictionary *mDict = [NSMutableDictionary dictionary];
        // 插入一个键值对
        [mDict setObject:@"www.baidu.com" forKey:@"website"];
        [mDict setObject:@"leo" forKey:@"name"];
        NSLog(@"website: %@", mDict[@"website"]); // "www.baidu.com"
        NSlog(@"name: %@", mDict[@"name"]);  // "leo"
    }
    return 0;
}
```

#### 修改键值对的值

可变字典除了能够新增键值对之外，还可以对已经存在的键值对进行修改，当需要修改时，我们需要更加键Key取出字典中的键值对，然后使用赋值运算符更新值 Value；

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 实例化一个可变字典
        NSMutableDictionary *mDict = [NSMutableDictionary dictionary];
        // 插入一个键值对
        [mDict setObject:@"www.baidu.com"  forKey:@"website"];
        [mDict setObject:@"leo" forKey:@"name"];
        NSLog(@"website值：%@", mDict[@"website"]); // www.baidu.com
        NSLog(@"name值：%@", mDict[@"name"]); // leo
        
        // 修改键值对的值
        mDict[@"website"] = @"www.apple.com";
        mDict[@"name"] = @"antony"; // 是否可以修改为 NSNumber?
        NSLog(@"website新值：%@", mDict[@"website"]); // www.apple.com
        NSLog(@"name新值：%@", mDict[@"name"]); // antony
    }
    return 0;
}
```

#### 移除键值对

当可变字典中的某个 / 某些键值不再需要时，可以使用 NSMutableDictionary 中定义的键值移除方法进行删除操作。

* removeObjectForKey 方法，可以移除某个键值对

```objectivec
- (void)removeObjectForKey:(KeyType)aKey;
```

* removeObjectForKeys 方法：可以移除多个键值对，把需要移除的所有Key 存储在一个数组对象中，作为参数传入。

```objectivec
- (void)removeObjectForKeys:(NSArray<keyType> *)keyArray;
```

* removeAllObjects 方法：可移除可变字典中的所有键值对

```objectivec
- (void)removeAllObjects;
```



