# NSFileManager类：文件操作

#### 基本概念

在学习 NSFileManager 类的相关属性和方法之前，需要提前了解并掌握如下几个与文件相关的基本概念。

* 路径\(Path\)：在使用 NSFileManager 类对文件进行操作时，经常需要使用到 路径 的概念，路径我们可以理解为文件的 物理存储路径 + 文件名 的组合，每个路径名都是一个 NSString 类型的对象。
* 属性\(attr\)：文件的属性是一个NSDictionary 类型的对象，文件属性定义在 Foundation/NSFileManager.h 文件中，大概有20多个。
* 错误\(error\)：一个指向 NSError 对象的指针，能提供有关操作更多的错误信息，如果 error 被置为 nil，那么就会采取默认的错误处理行为。

#### 基本操作

在使用 NSFileManager 类时，需要实例化一个 NSFileManager 类的对象，然后对指定路径 Path 上的文件进行一些操作。下方代码，演示如何获取目录，如何获取路径，如何实例化 NSFileManager 类，以及如何判断一个文件是否存在。

* 首先在电脑桌面上创建一个 myfile.txt 文件，可以打开终端，执行如下命令：

```text
cd $HOME/Desktop/
touch myfile.txt
```

* 文件准备完成后，在 main\(\) 函数中添加如下代码，需要注意的是：文件的路径可以通过两种方式来获取，第一种是直接给出绝对路径，另外一种是通过 NSSearchPathForDirectoriesInDomains\(\) 函数来获取。

```objectivec
int main(int argc, const char * argv[]){
    @atuoreleasepool {
        // 目录路径Path：绝对路径
        NSString *directoryPath = @"/Users/xiacan/Desktop";
        // 通过NSSearchPathForDirectoriesInDomains() 函数获取路径
        NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDesktopDirectory, NSUserDomainMask, YES);
        NSString *desktopPath = [paths objectAtIndex:0];
        
        // 文件路径
        // 注意：拼接文件名称需要使用 stringByAppendingPathComponent: 方法
        NSString *filePath1 = [dirctoryPath stringByAppendingPathComponent:@"myfile.txt"];
        NSString *filePath2 = [desktopPath stringByAppendingPathComponent:@"myfile.txt"];
        
        // 实例化 NSFileManager 对象
        NSFileManager *fm = [NSFileManager defaultManager];
        
        // 判断路径文件是否存在
        if ([fm fileExistsAtPath]:filePath1) {
            NSLog(@"myfile.txt exist filePath1");
        }
        if ([fm fileExistsAtPath:filePath2]) {
            NSLog(@"myfile.txt exist in desktopPath");      
        }
    }
    return 0;
}
```

#### 文件的复制

```objectivec
- (BOOL)copyItemAtPath:(NSString *)srcPath toPath:(NSString *)dstPath error:(NSError **)error;
```

#### 移动文件：除了移动文件外，还可以用于文件改名

```objectivec
- (BOOL)moveItemAtPath:(NSString *)srcPath toPath:(NSString *)dstPath error:(NSError **)error;
```

 删除文件

```objectivec
- (BOOL)removeItemAtPath:(NSString *)path error:(NSError **)error;
```

示例

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 目录路径Path：绝对路径
        NSString *directoryPath = @"/Users/xiacan/DeskTop";
        // 文件路径
        NSString *filePath = [directoryPath stringByAppendingPathComponent:@"myfile.txt"];
        // 实例化 NSFileManager 对象
        NSFileManager *fm = [NSFileManager defaultManager];
        
        // 复制文件
        NSString *copyFilePath = [directoryPath stringByAppendingPathComponent:@"myfilecopy.txt"];
        
        if (![fm fileExistsAtPath:copyFilePath]) {
            if([fm copyItemAtPath:filePath toPath:copyFilePath error:nil]) {
                NSLog(@"file copy success!");
            }
        }
        
        // 移动文件：除了移动文件外，还可以用于文件改名
        NSString *moveFilePath = [directoryPath stringByAppendingPathComponent:@"myfileNEWcopy.txt"];
        if ([fm fileExistsAtPath:filePath]) {
            if ([fm moveItemAtPath:filePath toPath:moveFilePath error:nil]) {
                NSLog(@"file move success");
            }
        }
        
        // 删除文件
        if ([fm removeItemAtPath:moveFilePath error:nil]) {
            NSLog(@"file remove success");
        }
    }
}
```

#### 获取和修改文件属性

每个文件都有一些其自身属性，例如：文件大小，文件类型，文件所有者等，我们可以使用 NSFileManager 来读取以及修改指定路径上文件的属性。

* 获取文件属性：可以使用 attributesOfItemAtPath 方法来获取文件的属性，返回值是一个字典，其中存储了该目标文件的属性。

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        //目录路径Path：绝对路径
        NSString *directoryPath = @"/Users/xiacan/Desktop";
        //文件路径
        NSString *filePath = [directoryPath stringByAppendingPathComponent:@"myfile.txt"];
        //实例化NSFileManager对象
        NSFileManager *fm = [NSFileManager defaultManager];
        
        NSDictionary *fileAttr = [fm attributesOfItemAtPath:filePath error:nil];
        NSLog(@"file owner name: <%@>, file create date:<%@>",fileAttr[NSFileOwnerAccountName],fileAttr[NSFileCreationDate]);
    }
    return 0;
}
```

文件的属性列表可以在 Foundation/NSFileManager.h 文件中查询，常用的一些属性如下所示。

```objectivec
NSFileAttributeKey const NSFileType; // 文件类型
NSFileAttributeKey const NSFileSize; // 文件大小
NSFileAttributeKey const NSFileCreationDate; // 文件创建日期
NSFileAttributeKey const NSFileModificationDate; // 文件修改日期
NSFileAttributeKey const NSFileOwnerAccountName; // 文件所有人
```

#### 更改文件属性

使用 setAttributes:ofItemAtPath:error 方法来设置文件属性，在调用该方法之前，需要把希望修改的属性，封装在一个字典里。

```objectivec
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        //目录路径Path：绝对路径
        NSString *directoryPath = @"/Users/shixin/Desktop";
        //文件路径
        NSString *filePath = [directoryPath stringByAppendingPathComponent:@"myfile.txt"];
        //实例化NSFileManager对象
        NSFileManager *fm = [NSFileManager defaultManager];
        //更改文件属性
        NSDictionary *attrDict = [NSDictionary dictionaryWithObjectsAndKeys:[NSDate distantFuture], NSFileCreationDate, nil];
        [fm setAttributes:attrDict ofItemAtPath:filePath error:nil];
        NSDictionary *fileAttr1 = [fm attributesOfItemAtPath:filePath error:nil];
        NSLog(@"file create date：<%@>", fileAttr1[NSFileCreationDate]);
    }
    return 0;
}

```

