### OC

```objective-c
// 导入基础框架
#import <Foundation/Foundation.h>

// 自动释放池
@autoreleasepool {}
```

### 类

```objective-c
@interface 类名 : NSObject {
    属性;
}
- (返回值类型) 方法名;
- (返回值类型) 方法名:(参数类型)参数名;
// 自动生成属性get/set方法声明
@property 属性;
@end
    
@implementation 类名
- (返回值类型) 方法名 {}
- (返回值类型) 方法名:(参数类型)参数名{}
- (返回值类型) 方法名:(参数类型1)参数名1 :(参数类型2)参数名2{}
// 类方法
+ (返回值类型) 方法名 {}
// 自动生成属性get/set方法实现
@synthesize 属性;
@end
    
// 创建对象
类名 *对象名 = [类名 new];
对象名 -> 属性 = 属性值;
(*对象名).属性 = 属性值;
[对象名 方法名];
[对象名 方法名:参数];
[对象名 方法名:参数1 :参数2];

// 分组导航标记
# pragma mark 标记名
// 分割线
# pragma mark -
```