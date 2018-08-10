# NSValue

#### 值对象的概念

在面向对象的编程中， 值对象本质是数据元素的对象包装器，所谓数据元素，常见的包含string，number，date类型以及其它自定义的结构体类型。OC语言本身提供了string，number，date相对应的包装类，分别是NSString，NSNumber，NSDate，这些类创建的对象都可以称为值对象。但值对象本身的范围更加广泛，它可以是任何自定义类型创建的对象。

#### 值对象作用

C语言提供了 char / int / float / double 基本数据类型，基于 C 语言的 OC 因此包含了这几种基本数据类型，我们可以定义并使用这些基本数据类型，也可以使用其值对象。对于自定义数据类型，我们也可以由这些类型定义的变量通过 NSValue 来包装成对象类型。相对于普通类型， 值对象提供更多的功能和作用。

* 可将任何值对象存储在集合中

在 OC 中， 诸如NSArray，NSDictionary 这样的集合类所包含的元素必须是对象类型。因此基本数据类型的变量必须转换为值对象才能存储在集合中。

* 更加丰富的数据处理办法

NSString 或 NSMutableString 可以进行一系列针对字符串的操作。如连接，分割，查找，比较等等。

NSDate 和 NSCaleder 可以进行复杂的日期处理和计算，所有这些计算都考虑了时区和闰年的影响。

NSNumber 和 NSDecimalNumber 可以处理 char，short int，int，long int，long long int，float or double，BOOL值，并提供了数值与字符串的转换。

#### NSValue释义

上面我们已经提到NSValue 可以包装基本数据类型为对象类型，下面我们看看官方文档的释义：

{% hint style="info" %}
 NSValue提供了简单的容器来包含C或Objective-C数据项。可以容纳任何基本数据类型如char，int，float，double，以及指针，结构体和对象ids。NSArray和NSSet集合类对象要求它们的元素为对象类型，NSValue的主要目的是使这些数据类型可以添加至集合中。NSValue对象是不可变类型。 
{% endhint %}

 简而言之，NSValue是基本数据类型或自定义数据类型所定义变量的对象包装器。 

#### 使用 NSValue

* 处理 NSRange 方法

```objectivec
+ (NSValue *)valueWithRange:(NSRange)range
- (NSRange)rangeValue
```

```objectivec
NSRange rangeA;

rangeA.location = 0;
rangeA.location = 10;

// 创建 NSRange 的值对象
NSValue *rangeValue = [NSValue valueWithRange:rangeA];

// 重新获取值对象包含的值
NSRange rangeB = [rangeValue rangeValue];
NSLog(@"%d,%d", rangeB.location, rangeB.length); // 10, 10
```

* 处理自定义结构体类型方法

```objectivec
+ (NSValue *)valueWithBytes:(const void *)value objCtype:(const char *)type;
- (id)initWithBytes:(const void *)value objcType:(const char *)type;
- (void)getValue:(void *)buffer;
```

```objectivec
// 结构体定义

typedef struct {
    int a;
    float b;
}DataItem;

DataItem dataElemA;
dataElemA.a = 10;
dataElemA.b = 10.005;
NSValue *value = [NSValue valueWithBytes: &dataItem objCType:@encode(DataItem)];
DataItem dataElemB;
[value getValue:@dataElemB];
NSLog(@"%d,%0.3f", dataElemB.a, dataElemB.b); // 10，10.005
```

自定义类型必须是固定长度类型，不能将C 字符串， 可变长度的数组和结构体，以及其他变长类型存储在 NSValue 中， 这些可变类型应该使用 NSString 或 NSData 来包装成对象类型。但可以将可变数据类型的指针保存在 NSValue  中，官方文档示例：

 原意想要保存myCString到NSValue中，但实际上myCString是以char的指针类型进行解析的，所以字符串的前四个字节被当做了指针的值，而不是地址值来对待。 

```objectivec
char *myCString = "This is a string.";
NSValue *theValue = [NSValue valueWithBytes:myCString objCType:@encode(char *)];
char *cc = (char*)malloc(sizeof(char*)*200);
[theValue getValue:cc];
printf("%s", cc); // This
free(cc);
```

正确的做法是保存字符串到 NSString 中，如：

```objectivec
char *myCString = "This is a string";
NSString myNsString = [NSString stringWithCString:myCString encoding:NSUTF8StringEncoding];
```

或者，保存该字符串的指针地址到 NSValue 中，如：

```objectivec
char *myCString = "This is a string";
NSValue *theValue = [NSValue valueWithBytes:&myCString objCType:@encode(char **)];
char **cc = (char**)malloc(sizeof(char**)*200);
[theValue getValue:cc];
printf("---------%s---------", *cc); // This is a string.
free(cc);
cc = NULL;
```

处理指针类型。方法：

```objectivec
+ (NSValue *)valueWithPointer:(const void *)aPointer
- (void *)pointerValue
```

如：

```objectivec
DataItem *dd = (DataItem*)malloc(sizeof(DataItem)) ;

dd->a = 1 ;

dd->b = 2 ;
    
NSValue *pValue = [NSValue valueWithPointer:dd] ;
    
DataItem *dc = (DataItem*)[pValue pointerValue] ;
    
NSLog(@"%d,%0.3f",dc->a,dc->b);
    
free(dd) ;
    
dd = NULL ;
    
dc = NULL ;
```



#### NSValue 的分类

 [UIKit Additions](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/NSValue_UIKit_Additions/Reference/Reference.html) 

提供了 Fundaction 框架中关于集合数据类型结构体的对象值包装，包括  CGPoint,CGRect,CGSize,CGAffineTransform,UIEdgeInsets,UIOffset。

 [Core Animation Additions](https://developer.apple.com/library/ios/#documentation/GraphicsImaging/Reference/NSValue_CA_additions/Introduction/Introduction.html) 

 提供CATransform3D结构体的对象值包装

 [MapKit Additions](https://developer.apple.com/library/ios/#documentation/MapKit/Reference/NSValue_MapKit_additions/Reference/Reference.html)  
  
        提供[CLLocationCoordinate2D](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocationDataTypesRef/Reference/reference.html#/apple_ref/doc/c_ref/CLLocationCoordinate2D)和[MKCoordinateSpan](https://developer.apple.com/library/ios/documentation/MapKit/Reference/MapKitDataTypesReference/Reference/reference.html#/apple_ref/doc/c_ref/MKCoordinateSpan)结构体的对象包装

