---
title: 一行代码搞定TextField输入限制
date: 2017.09.18 10:47
---

网上有很多介绍输入限制的文章
原理请参看[这篇文章](http://www.jianshu.com/p/2d1c06f2dfa4)，我只是做了一个简单的封装而已

这里可以贴出全部代码

```
#import <UIKit/UIKit.h>

@interface UITextField (TMRInputLimit)
- (void)inputLimitWithMaxLength:(NSInteger)maxLength;
@end
```


```
#import "UITextField+TMRInputLimit.h"
#import <objc/runtime.h>
@interface UITextField ()
@property (assign, nonatomic) NSNumber *maxLength;
@end
@implementation UITextField (TMRInputLimit)

- (void)setMaxLength:(NSNumber *)maxLength
{
    objc_setAssociatedObject(self, @selector(maxLength), maxLength, OBJC_ASSOCIATION_ASSIGN);
}

- (NSNumber *)maxLength
{
    return objc_getAssociatedObject(self, @selector(maxLength));

}

- (void)inputLimitWithMaxLength:(NSInteger)maxLength
{
    self.maxLength = [NSNumber numberWithInteger:maxLength];
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(textFieldEditChanged:) name:@"UITextFieldTextDidChangeNotification" object:self];
}

- (void)textFieldEditChanged:(NSNotification *)obj
{
    NSInteger maxLength = [self.maxLength integerValue];
    UITextField *field = (UITextField *)obj.object;
    NSString *toBeString = field.text;
    UITextRange *selectedRange = [field markedTextRange];
    UITextPosition *position = [field positionFromPosition:selectedRange.start offset:0];
    if (!position || !selectedRange)
    {
        if (toBeString.length > maxLength)
        {
            NSRange rangeIndex = [toBeString rangeOfComposedCharacterSequenceAtIndex:maxLength];
            if (rangeIndex.length == 1)
            {
                field.text = [toBeString substringToIndex:maxLength];
            }
            else
            {
                NSRange rangeRange = [toBeString rangeOfComposedCharacterSequencesForRange:NSMakeRange(0, maxLength)];
                field.text = [toBeString substringWithRange:rangeRange];
            }
        }
    }
    
}

- (void)dealloc
{
    [[NSNotificationCenter defaultCenter] removeObserver:self name:@"UITextFieldTextDidChangeNotification" object:self];
}

@end

```

使用时import这个分类

```
[_textField inputLimitWithMaxLength:6];
```


