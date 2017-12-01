---
title: iOS 获取当前正在显示的ViewController
date: 2017-09-12 12:13:13
categories:
    - iOS
tags:
    - iOS
    - 记录
---

### iOS 获取当前正在显示的ViewController,方法有如下几种
--- --- --- 
#### 1. 从UIWindow中获取
```objc
#import "UIWindow+SHHelper.h"  
  
@implementation UIWindow (SHHelper)  
  
- (UIViewController*)sh_topMostController  
{  
    //  getting rootViewController  
    UIViewController *topController = [self rootViewController];  
      
    //  Getting topMost ViewController  
    while ([topController presentedViewController]) topController = [topController presentedViewController];  
      
    //  Returning topMost ViewController  
    return topController;  
}  
  
- (UIViewController*)sh_currentViewController;  
{  
    UIViewController *currentViewController = [self sh_topMostController];  
      
    while ([currentViewController isKindOfClass:[UINavigationController class]] && [(UINavigationController*)currentViewController topViewController])  
        currentViewController = [(UINavigationController*)currentViewController topViewController];  
      
    return currentViewController;  
}  
  
@end  
```

#### 2. 从UIView里面获取
```objc
//满足一个日常的需求:在UITableviewcell里面的UIView模块里面，调用self.navigationcontroller pushviewcontroller推入一个新的viewcontroller，需要获取其上层的UIViewcontroller, 可以使用下面的方法:  
- (UIViewController *)sh_viewController  
{  
    UIResponder *responder = self;  
    while ((responder = [responder nextResponder])){  
        if ([responder isKindOfClass: [UIViewController class]]){  
            return (UIViewController *)responder;  
        }  
    }  
    return nil;  
}  
```
#### 3. 从UIViewController中获取
```objc
#import "UIViewController+SHHelper.h"  
  
@implementation UIViewController (SHHelper)  
  
- (UIViewController*)sh_topMostController  
{  
    UIViewController *topController = self ;  
      
    while ([self presentedViewController])  
          topController = [topController presentedViewController];  
      
    return topController;  
}  
  
- (UIViewController*)sh_currentViewController;  
{  
    UIViewController *currentViewController = [self sh_topMostController];  
      
    while ([currentViewController isKindOfClass:[UINavigationController class]] && [(UINavigationController*)currentViewController topViewController])  
        currentViewController = [(UINavigationController*)currentViewController topViewController];  
      
    return currentViewController;  
}  
  
//我们在非视图类中想要随时展示一个view时，需要将被展示的view加到当前view的子视图，或用当前view presentViewController，或pushViewContrller，这些操作都需要获取当前正在显示的ViewController。  
//获取当前view的UIViewController  
+ (UIViewController *)sh_currentViewControllerFromcurrentView{  
      
    UIViewController *result = nil;  
      
    // 1. get current window  
    UIWindow * window = [[UIApplication sharedApplication] keyWindow];  
    if (window.windowLevel != UIWindowLevelNormal) {  
        NSArray *windows = [[UIApplication sharedApplication] windows];  
        for(UIWindow * tempWindow in windows) {  
            if (tempWindow.windowLevel == UIWindowLevelNormal) {  
                window = tempWindow;  
                break;  
            }  
        }  
    }  
      
    // 2. get current View Controller  
    UIView *frontView = [[window subviews] objectAtIndex:0];  
    id nextResponder = [frontView nextResponder];  
    if ([nextResponder isKindOfClass:[UIViewController class]]) {  
        result = nextResponder;  
    } else {  
        result = window.rootViewController;  
    }  
    return result;  
}  
  
//获取当前屏幕中present出来的viewcontroller。  
- (UIViewController *)getPresentedViewController  
{  
    UIViewController *appRootVC = [UIApplication sharedApplication].keyWindow.rootViewController;  
    UIViewController *topVC = appRootVC;  
    if (topVC.presentedViewController) {  
        topVC = topVC.presentedViewController;  
    }  
      
    return topVC;  
}  
  
@end  
```

