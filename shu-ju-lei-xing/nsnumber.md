# NSNumber

#### NSNumber 对象创建

当我们需要把基本类型转换为 NSNumber 类型的对象时，转换方式通常有以下两种。

* 使用 @ 符号进行快速封装；
* 调用 NSNumber 类提供的方法，NSNumber类型的对象不仅可以转换 int，float，double 这些常规的基本数字类型， 还可以包装 char，BOOL，但并不常用。

```objectivec
+ (NSNumber *)numberWithInt:(int)value; // 转换int
+ (NSNumber *)numberWithFloat:(float)value; // 转换float
+ (NSNumber *)numberWithDouble:(double)value; // 转换double
+ (NSNumber *)numberWithBool:(BOOL)value; // 转换bool
+ (NSNumber *)numberWithInteger:(NSinteger)value; // 转换为NSInteger
```

下方的示例代码中演示了常用的基本数据类型转换为 NSNumber 对象的方法:

```objectivec
int main(int argc, const * argv[]) {
    @autoreleasepool {
        // NSNumber 对象创建
        // 快速创建
        NSNumber *intNum1 = @10;
        NSNumber *floatNum1 = @3.14;
        // 使用类方法创建
        NSNumber *intNum2 = [NSNumber numberWithInt:10];
        NSNumber *floatNum2 = [NSNumber numberWithFloat:3.14];
        NSNumber *integerNum = [NSNumber numberWithInteger:100];
        NUNumber *doubleNum = [NSNumber numberWithDouble:100.01];
        NSLog(@"%@----%@----%@----%@---%@-----%@", intNum1,intNum2,floatNum1,floatNum2,integerNum,doubleNum);
    }
    return 0;
}
```

打印结果：

```text
10---10---3.14---3.14---100---100.01
```

#### NSNumber 对象与基本数据类型之间的转换

基本数据类型可以转换为 NSNumber 之外，NSNumber对象也可以转为基本数据类型，在 NSNumber 中提供了相应的方法：

```objectivec
@property (readonly) int intValue; // NSNumber对象转为int
@property (readonly) float floatValue; // NSNumber对象转为float
@property (readonly) double doubleValue; //NSNumber对象转为 double
@property (readonly) BOOL boolValue; //NSNumber对象转为BOOL
@property (readonly) NSInteger integerValue; // NSNumber对象转为NSInteger
```

下方示例演示如何把NSNumber 对象转为其他基本类型

```objectivec
int main(int argc, const * argv[]) {
    @autoreleasepool {
        // 使用类方法创建
        NSNumber *intNum = [NSNumber numberWithInt:10];
        NSNumber *floatNum = [NSNumber numberWithFloat:3.14];
        NSNumber *integerNum = [NSNumber numberWithInteger:100];
        NSNumber *doubleNum = [NSNumber numberWithDouble:100.01];
        
        // 转换基本类型
        int intBasic = [intNum intValue];
        float floatBasic = [floatNum floatValue];
        double doubleBasic = [doubleNum doubleValue];
        NSInteger integerBasic = [integerNum integerValue];
        NSLog(@"%d---%f---%f---%ld", intBasic,floatBasic,doubleBasic,integerBasic);
    }
    return 0;
}
```

打印结果：

```text
10---3.140000---100.010000---100
```

#### 对比

* int：当使用 int 类型定义变量的时候，可以像写 C 程序一样使用。当你不知道程序运行在哪种处理器架构时，你最好使用 NSInteger，因为 int 在 32位系统中也许是 Int 类型，而在 64 位系统中 int 可能变成 long 型。
* NSInteger / NSUInteger 是一种动态定义的类型，在不同的设备，不同的架构，有可能是 int 类型，有可能是 long 类型。NSUInteger 是无符号的，即没有负数。NSInteger是有符号的。
* NSInteger 是基础类型，NSNumber 是一个类，如果需要在数组中存储一个数值，直接使用 NSInteger 是不行的，因为 OC 的集合只能存储对象！

