---
description: >-
  字典是由 键-值(key-value)组成的数据集合，其中值 value 为对象。可以通过键 (key) 从字典中获取需要的 value 值。字典中的键
  key 必须唯一，通常情况下，键(key) 为字符串对象，主要用于注明存储对象的说明，但键 key 也可以是其他类型的对象。和
  键(key)关联的值可以是任何对象类型，但不能是 nil。
---

# NSDictionary

#### 创建不可变字典

在 Foundaction 框架中， 提供了 NSDictionary 类，该类中定义了与字典操作相关的属性和方法。

NSDictionary 类是不可变字典，即字典创建完成后，不能再新增/删除键值对。创建不可变字典的方法,：

* 字面量创建的方式：

```objectivec
@{
    key1: value,
    key2: vlaue,
    ...
};
```

例如我们创建一个 NSDictionary 类的对象，其中存储4个键值对，这4个键值对的 key 都使用字符串对象，用来说明键值的含义，而value 中可以保存各种类型的对象，可以是 NSString 字符串对象，也可以是NSNumber 数字对象

```objectivec
int main(int argc, const * argv[]) {
    @autoreleasepool {
        // 快速创建字典
        NSDictionary *dict = @{
            @"website": @"www.baidu.com",
            @"name": "leo",
            @"business": "iso学习",
            @"age": @26
        }
    }
    return 0;
}
```

* 调用 dictionaryWithObjectsAndKeys 类方法，使用该方法创建字典时，注意顺序是： value，key，并且用  nil 结尾。如下创建的字典：

```objectivec
int main(int argc, const * argv[]) {
    @autoreleasepool {
        // 快速创建字典
        NSDictionary *dict = [NSDictionary dictionaryWithObjectsAndKeys:
                                    @"www.baidu.com",
                                    @"website",
                                    @"leo",
                                    @"name",
                                    nil];
    }
    return 0;
}
```

* 调用 dictionaryWithObjects: forKeys 方法，创建字典

```objectivec
int main(int argc, const * argv[]) {
    @autoreleasepool {
        // 创建多组键值对的字典
        NSArray *keys = @[@"website", @"name"];
        NSArray *values = @[@"www.baidu.com", @"l"]
        NSDictionary *dict = [NSDictionary dictionaryWithObjects:values forKeys:keys];
    }
    return 0;
}
```

{% hint style="info" %}
Key 通常为 NSString 类型，但也可以为其他类型，如下创建的字典中，键key 为一个 NSNumber 类的对象，这在语法上是允许的，但实际开发中一般不这样使用。
{% endhint %}

```objectivec
// 创建非NSString 类型的key
NSDictionary *dict = @{
    @2016: @"2016"
};
```

#### 访问字典的键值

字典最常用的操作就是根据键key 来获取对应的值value。通常情况下，由如下两种方式来取值。

* 使用 dict\[key\] 的形式
* 使用 objectForKey 方法

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSDictionary *dict = @{
                               @"website":@"www.baidu.com",
                               @"name":@"leo",
                               @"business":@"iOS学习",
                               @"foundedYear":@2016
                               };
        NSString *website = dict[@"website"];
        NSLog(@"字典中website对应的value: %@",website);
        NSString *name = [dict objectForKey:@"name"];
        NSLog(@"字典中name对应的value: %@",name);
    }
    return 0;
}
```

#### 遍历字典中的键值对

在 OC 中提供了 forin 循环，forin 循环除了能够用于遍历数组中的对象之外，也可以用于遍历字典中的键值对。

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 快速创建字典
        NSDictionary *dict = @{
            @"websit": @"www.baidu.com",
            @"name": @"leo",
            @"business": @"ios学习",
            @"age": @26
        }
        // 枚举字典
        for (NSDictionary *key in dict) {
            NSLog(@"key: %@ ---------- value: %@", key, dict[key]);
        }
    }
    return 0
}
```

#### 其他操作

对于字典的操作，除了上面介绍的字典创建以及键值对的取值之外，还有如下几个在 NSDictionary 类中定义的属性或方法相对常用。

* 获取字典中键值对的数量

```objectivec
@property (readonly) NSUInterger count;
```

* 获取一个字典中所有的键Key，返回一个数组

```objectivec
@property (readonly) NSArray<KeyType> *allKeys;
```

* 获取一个字典中所有的值value，返回一个数组

```objectivec
@property (readonly, copy) NSArray<ObjectType> *allValues;
```



