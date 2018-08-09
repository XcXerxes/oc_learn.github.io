---
description: 继承是面向对象最显著的一个特性，继承是从已有的类中派生一个新的类， 新的类拥有已有类的所有属性和行为， 同时能扩展新的属性和行为。
---

# 继承

### 为什么有继承

假如有两个类，分别是教师类和学生类，定义如下：

```objectivec
#import <Foundation/Foundation.h>

@interface Teacher: NSObject {
    NSString *_name;
    NSInteger _age;
    float _height;
    float _salary;
}

@end
```

```objectivec
#import <Foundation/Foundation.h>

@interface Student: NSObject {
    NSString *_name;
    NSInteger _age;
    float _height;
    float _score;
}

@end
```

学生类和教师类都有姓名，年龄和身高。那么， 可不可以想个办法，把这些成员提取出来，作为一个新的类，然后让学生类和教师类都引入就行？这里就需要使用到继承了。

### 怎么继承

对于上面的问题

* 对于 `Student`和 `Teacher` 两个类来说， 首先找到他们公共的地方\(姓名，年龄，身高\);
* 定义一个保存它们共同部分的新类；

定义一个新的类 `Person`

```objectivec
#import <Foundation/Foundation.h>

@interface Person: NSObject {
    NSString *_name;
    NSInteger _age;
    float _height;
}

@end
```

然后，学生有自己独有的成员变量，叫做 `_score`，写法如下：

```objectivec
#import <Foundation/Foundation.h>
#import "Person.h"

@interface Student: Person {
    float _score;
}

@end
```

教师类拥有独有的成员变量， 叫做 `_salary`，写法如下：

```objectivec
#import <Foundation/Foundation.h>
#import "Person.h"

@interface Teacher: Person {
    float _salary;
}

@end
```

### 继承与方法

与上面使用继承一样， 对于 `Person` 类，也有自己的初始化方法，比如 `init` 来初始化成员变量， 定义和实现如下：

```objectivec
#import <Foundation/Foundation.h>

@interface Person: NSObject {
    NSString *_name;
    NSInteger _age;
    float _height;
}

- (instancetype) initWithName:(NSString *)name age: (NSInteger) age height: (float) height;

@end

```

方法的实现：

```objectivec
#import "Person.h"

@implementation Person

- (instancetype) initWithName:(NSString *)name age:(NSInteger) age height:(float)height {
    self = [super init];
    if (self) {
        _name = name;
        _age = age;
        _height = height;
    }
    return self;
}

@end
```

同样，因为 `Student` 与 `Teacher` 继承自 `Person`，所有他们也可以直接调用自定义 `init` 方法来进行对象的初始化。

```objectivec
#import <Foundation/Foundation.h>
#import "Person.h"
#import "Student.h"
#import "Teacher.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...
        
        Person *person = [[Person alloc] initWithName: @"A" age: 14 height: 140];
        Teacher *teacher = [[Teacher alloc] initWithName: @"B" age: 15 height: 150];
        Student *student = [[Student alloc] initWithName: @"C" age: 16 height: 155];
    }
    
    return 0;
}

```

### 访问权限修饰符

{% hint style="info" %}
权限修饰符是用来修饰实例变量的， 用来控制实例变量的访问权限。
{% endhint %}

常用的访问权限修饰符有：`@public`，`@protected`，`@private`，区别如下：

| 修饰符名称 | 修饰后的变量作用域 | 其他说明 |
| :--- | :--- | :--- |
| @public | 所有地方都能访问 | 少用， 容易破坏封装性 |
| @protected | 在本类中和子类中可以访问 | 系统默认为@protected权限 |
| @private | 只能在本类中访问，不能继承给子类 | —————— |

修饰符的使用

```objectivec
#import <Foundation/Foundation.h>

@interface Person: NSObject {
    // 表示_name与_age两个成员变量是公开的，任何地方都可以访问
    @public
    NSString *_name;
    NSInteger _age;
    
    // 表示_height 成员变量是私有的， 只能自己访问， 不能被子类访问
    @private
    float _height;
}

@end
```

{% hint style="warning" %}
继承的注意事项

* OC中的继承为**单继承，**一个子类， 只能有一个父类**。**
* 被继承的类成为**父类（或超类）**，继承其他类的类， 称为**子类**。
{% endhint %}



