---
description: NSDate对象用来处理时间日期的，它存储的是世界标准时间，所以使用的时候需要根据不同的时区，将世界世界转换为本地的时间。
---

# NSDate

#### 获取日期和时间

NSDate 类中提供了 date 方法，用来获取当前标准时区的时间。在通常情况下，我们需要把标准时间转为本地时间。下方示例代码演示如何获取当前时间及如何将标准时间转换为本地时间。

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 获取时间
        NSDate *date = [NSDate date];
        // 打印标准时间
        NSLog(@"国际标准时间：%@", date);
        
        // 转为本地时间
        // 获取系统当前区
        NSTimeZone *zone = [NSTimeZone systemTimeZone];
        // 获取当前时区与格林尼治时间的间隔
        NSInteger interval = [zone secondsFromGMTForDate:date];
        // 获取本地时间
        NSDate *localDate = [NSDate dateWithTimeIntervalSinceNow:interval];
        NSLog(@"当前时区时间：%@", localDate);
    }
    return 0; 
}
```

#### 其他常用操作

除了创建NSDate 日期对象以及对日期对象的格式进行转换之外，NSDate 类中还有如下几个方法也相对常用。

* 比较两个时间对象的先后，可以使用 earlierDate 以及 laterDate 方法\*

```objectivec
- (NSDate *)earlierDate:(NSDate *)anotherDate;
- (NSDate *)laterDate:(NSDate *)antherDate;
```

* 比较两个时间的间隔，可以使用 timeIntervalSinceDate 方法，同时由于程序运行耗时的原因，会有微小的误差：

```objectivec
- (NSTimeInterval)timeIntervalSInceDate:(NSDate *)anotherDate;
```

* 比较两个日期是否相等，可以使用 isEqualToDate 方法

```objectivec
- (BOOL)isEqualToDate:(NSDate *)otherDate;
```

下方的示例代码中，演示了日期对象之间进行各种比较操作。

```objectivec
void dateOperation () {
    NSDate *date1 = [NSDate date];
    NSDate *date2 = [NSDate dateWithTimeIntervalSinceNow:10];
    
    // 比较两个时间的先后
    NSDate *tempDate = [[NSDate alloc] init];
    // 返回比较早的那个时间
    tempDate = [date1 earlierDate:date2];
    NSLog(@"earlierDate：%@", tempDate);
    
    // 返回比较晚的那个时间
    tempDate = [date1 laterDate:date2];
    NSLog(@"laterDate: %@", tempDate);
    
    // 比较两个时间的间隔(程序运行耗时的原因，会有微小的误差)
    NSTimeInterval timeInterval = [date2 timeIntervalSinceDate:date1];
    NSLog(@"interval = %f", timeInterval);
    
    // 比较两个日期是否相等
    if ([date1 isEqualToDate:date2]) {
        NSLog(@"date1 与 date2 相同");
    } else {
        NSLog(@"date1 与 date2 不同");
    }
}
```

