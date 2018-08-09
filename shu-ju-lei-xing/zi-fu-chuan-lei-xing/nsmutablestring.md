# NSMutableString

[NSMutableString ](nsmutablestring.md)继承自 [NSString](nsstring.md)。之前介绍的 [NSString](nsstring.md)  为不可变字符串，任何操作都不能对原字符串进行修改。而 NSMutableString 不同，作为可变字符串，可直接对原字符串进行编辑。

{% hint style="warning" %}
因为 [NSMutableString](nsmutablestring.md) 继承自 [NSString](nsstring.md)，所以 [NSString](nsstring.md)  的方法 [NSMutableString](nsmutablestring.md) 依然可用。他们最大的区别就是， [NSString](nsstring.md) 会返回新的字符串， 而 [NSMutableString](nsmutablestring.md) 对原字符串处理， 返回 `void`。
{% endhint %}

对于 [NSMutableString](nsmutablestring.md) 也有很多常用的方法， 本讲将介绍：

* [初始化](nsmutablestring.md#chu-shi-hua)
* [添加](nsmutablestring.md#tian-jia-zi-fu-chuan) / [删除](nsmutablestring.md#shan-chu-zi-fu-chuan) / [修改](nsmutablestring.md#xiu-gai-zi-fu-chuan)

#### 初始化

使用 [NSString](nsstring.md) 的几种初始化方法，依然可以对 [NSMutableString](nsmutablestring.md) 有效，写法如下：

```objectivec
NSMutableString *string1 = [NSMutableString string];
NSMutableString *string2 = [NSMutableString stringWithFormat:@"abc"];
```

还有两种独有的

```objectivec
// 给出初始长度，进行字符串的初始化 (这个初始长度不用太准确，因为系统分配的内容会随着字符串长度而变动)
NSMutableString *string3 = [[NSMutableString alloc] initWithCapacity: 4];
NSMutableString *string4 = [NSMutableString stringWithCapacity: 4];
```

#### 添加字符串

```objectivec
[string2 appendString:@"def"]; // 从尾部追加字符串@"def"
[string2 appendFormat:@"%d", 9]; // 从尾部格式化追加字符串@"9"
[string2 insertString:@"000", atIndex: 0]; // 从第0位插入字符串@"000"
```

#### 删除字符串

```objectivec
// 从0开始，删除3个字符
[string2 deleteCharactersInRange:NSMakeRange(0, 3)];
```

#### 修改字符串

```objectivec
// 按范围替换 从0 -3 替换位 "vvv"
[string2 replaceCharactersInRange:NSMakeRange(0, 3) withString:@"vvv"];

// 完全替换 将整个字符串替换位 "333"
[string2 setString:@"333"];
```



