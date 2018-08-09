# 方法

### 什么是方法

OC 中的方法 类似于 C语言中的函数，他们的区别在于， 函数没有属于的关系， 谁都可以直接调用，而方法有属于关系， 一般会说， 这个方法是属于某个类或某个对象的。

{% hint style="info" %}
方法：对象或类的行为。

学生写作业这个行为，`写作业` 就是一个方法；

老师讲课这个行为，`讲课` 就是一个方法；
{% endhint %}

### 怎么写方法

#### 方法申明的格式和含义

```text
方法类型 (返回值类型) 方法名With参数描述：(参数类型)参数名 参数描述：(参数类型)参数名;
```

* 在 OC 中有两种方法，一种以 — 开头， 一种以 ＋ 开头。— 开头为实例方法，用对象来进行调用；+ 为类方法\(静态方法\)，以类名调用；
* 参数的传递使用：来传递；
* 方法名和参数描述之间一般用 with 来进行连接\(这只是编码规范\)；
* 多个参数用 空格 隔开， 然后添加第二个参数描述与参数名；
* 方法名一般采用驼峰命名。

方法的声明：

```objectivec

- (instancetype)initWithName:(NSString *)name age:(NSInteger)age
@end
```

方法的实现：

```objectivec
- (instancetype)initWithName:(NSString *)name age:(NSInteger)age {
    // 这里写实现内容...
}
@end
```



### 方法的使用与调用

#### 方法的调用

在初始化 `Person` 对象的时候，我们使用到了以下代码：

```text
Person *preson1 = [Person alloc];
person1 = [person1 init];
```

按住 `command` 键，点击 alloc 可以进入到 `NSObject` 的头文件中，找到 `alloc` 与 `init` 方法的申明：

```objectivec
+ (instancetype)alloc;  // 开辟内存
- (instancetype)init;   // 初始内存
```

* 方法使用 空格 来调用；
* `+` 方式使用类名进行调用，如这里的 `[Person alloc]`；
* `—` 方式使用对象进行调用，如这里的 `[person1 init]`;

#### 对象的初始化

刚才初始化 `Person` 对象的时候，我们使用的是 `init` 方法， 但是这样 `init` 出来的对象没有自身独有的元素。比如， 我们的 `Person` 类中， 还有姓名，年龄等成员变量根本体现不出来。如果想要对某个人的姓名，年龄赋值，怎么办？

#### 自定义初始化方法

首先在 `.h` 的 `@interface` 内部申明方法（如果不申明，对外不可访问）：

```objectivec
#import <Foundation/Foundation.h>

@interface Person: NSObject {
    NSString *_name;
    NSInteger _age;
    float _height;
}

// 注意这里的规范，先定义成员变量，然后换行再申明方法
- (instancetype)initWithName:(NSString *)name age:(NSInteger) age height:(float)height;

@end
```

在 `.m` 中进行实现：

```objectivec
#import "Person.h"

@implementation Person 

- (instancetype)initWithName:(NSString *)name age:(NSInteger)age height:(float)height{
    self = [super init];
    if (self) {
      // 需要添加的特性
      _name = name
      _age = age
      _height = height
    }
    return self;
}

@end
```

在自定义初始化方法的时候，一定要注意以下几点：

* 方法名必须是 `initWith` 开头，注意大小写；
* 每个 `init` 方法的基本结构如下；

```objectivec
- (instancetype)initWithXxx:(NSString *)xxx {
    slef = [super init];
    
    if (self) {
        // 需要添加的特性...
    }
    return self;
}

@end
```

那么这个时候，我们在 main.m 中的 Person 初始化方法也需要修改：

```objectivec
Person *person2 = [[Person alloc] initWithName:@"leo" age: 14 height: 120];
```

### description 方法

当使用 `NSLog` 打印对象信息的时候， 系统会走该对象的 `description` 方法。

比如这里， 我们想要输出 `person2` 这个人的信息，就可以通过重写 `description` 来实现

### 方法的重写

在对象初始化的时候，我们提到一个疑问：

{% hint style="warning" %}
初始化 Person 对象的时候，我们所使用的是 init 方法，但是这样 init 出来的对象没有自身独有的元素。比如， 我们的 Person 类，还有姓名，年龄等成员体现不出来。如果想要对某个人的姓名，年龄赋值， 怎么办？
{% endhint %}

当时给出的解决方案是：自定义一个新的 Init 方法，将姓名与年龄当参数传进去。

这一节， 提供新的解决方案：重写 Init 方法。

#### 重写 Init 方法

在子类中修改父类的方法，我们称之为 重写 。

* 重写方法，不需要再在 .h 文件中进行申明，直接在 .m 文件中实现即可;
* 父类必须有这个方法，而且必须公开\(方法在父类的 .h 中申明过\)
* 重写时，只能修改方法的实现。方法类型，返回值，方法名，参数都不能修改。

```objectivec
- (instancetype) init {
    self = [super init];
    if (self) {
        _name = @"Exo";
        _age = 1;
        _height = 0.5;
    }
    return self
}

```

这时，我们再调用 init 方法，就不仅仅是走 NSObject 的方法了， 还会走重写过后的 init 方法。所以输出 _age_ 与 \_height 的值就是 1 与 0.5。

```text
Person *person1 = [[Person alloc] init];
```

#### 重写 description 方法

之前讲到，要打印对象， NSLog 中需要使用 %@ 。既然如此， person1 也是对象， 也可以使用 %@ 输出。

```text
NSLog(@"%@", person1);
```

控制台打印对象的地址

```text
<Person: 0x10020da60>
```

现在我想要 NSLog 对象的时候，控制台输出这个人的名字，就需要重写 description 方法， 实现如下：

```objectivec
- (NSString *)description {

    // 注意：_name这里的参数，一定不能换成self，会造成循环调用
    return [NSString stringWithFormat:@"姓名：%@", _name];
}

```

 这时再输出，控制台打印

```text
姓名：Karen
```

