# OC中的私有方法
##本小节知识点:
1. 【掌握】OC中的私有变量
2. 【掌握】OC中的私有方法

---

##1.OC中的私有变量
- 在类的实现即.m文件中也可以声明成员变量,但是因为在其他文件中通常都只是包含头文件而不会包含实现文件,所以在.m文件中声明的成员变量是@private的。在.m中定义的成员变量不能和它的头文件.h中的成员变量同名,在这期间使用@public等关键字也是徒劳的。

```objc
@implementation Dog
{
    @public
    int _age;
}
@end
```
---

##2.OC中的私有方法
- 私有方法：只有实现没有声明的方法
- 原则上：私有方法只能在本类的方法中才能调用。

    + >注意: OC中没有真正的私有方法

```objc
@interface Dog : NSObject

@end
@implementation Dog

- (void)eat
{
    NSLog(@"啃骨头");
}
@end
int main(int argc, const char * argv[]) {

    Dog *d = [Dog new];
    SEL s1 = @selector(eat);
    [d performSelector:s1];

    return 0;
}
```
