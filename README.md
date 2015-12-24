# ios-ui

主要会讲storyboard和xib


## storyboard和xib

- storyboard是故事版，强调的是连贯的情景
- xib更多的单一场景，比如不在场景里的viewcontroller或某些视图


storyboard能做xib的所有事情，但是我们还是推荐混用，代码结构上更加清晰，利用每种擅长的点即可

## get  a viewcontroller from storyboard

```
UIStoryboard *storyBoard = [UIStoryboard storyboardWithName:@"gallery" bundle:nil];
UIViewController *home_controller = [storyBoard instantiateInitialViewController];
```

## storyboard不适合团队开发？

- 如果全部功能放到一个storyboard写，肯定不适合，但分拆合理的话，确实是可以团队开发的。
- 扬长避短，比如自定义侧滑、tab等，但对应的vc里是完整路程，这种先手动代码，然后出发流程的地方加载storyboard即可


举个侧滑的REFrostedViewController的例子

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    
    UIStoryboard *storyBoard = [UIStoryboard storyboardWithName:@"gallery" bundle:nil];
    UIViewController *home_controller = [storyBoard instantiateInitialViewController];
    
    // Create content and menu controllers
    //
    DEMONavigationController *navigationController = [[DEMONavigationController alloc] initWithRootViewController:home_controller];
    TSMenuViewController *menuController = [[TSMenuViewController alloc] init];
    
    // Create frosted view controller
    //
    REFrostedViewController *frostedViewController = [[REFrostedViewController alloc] initWithContentViewController:navigationController menuViewController:menuController];
    frostedViewController.direction = REFrostedViewControllerDirectionLeft;
    frostedViewController.liveBlurBackgroundStyle = REFrostedViewControllerLiveBackgroundStyleLight;
    frostedViewController.liveBlur = YES;
    frostedViewController.delegate = self;
    
    // Make it a root controller
    //
    self.window.rootViewController = frostedViewController;
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    
     [[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:UIStatusBarAnimationFade];
    
    return YES;
}
```

## 拆分流程

但凡是是完整流程的尽量用storyboard做，且每一个流程使用一个storyboard，于是

![](img/3.png)

但是流程又分1个vc或多个vc，此时又会有很多单独的vc，它也是可以storyboard的。

## FAQ

### storyboard中怎么约束一个空间的长宽比

在控件自身上右键拖拽并松开，连到自己身上，会弹出一个加约束的菜单项，选择Aspect Ratio，然后在右边改成这样就行了

![](img/1.png)

注意firstItem 与secondItem及multipler的设置。例子中设置的长宽比为1：0.6 

### autolayout 如何通过约束设定长宽比

![](img/2.png)

前两个是设置到你视图左右边距为0，就是父视图的宽度，第三个是设置对应的比例


## 原则


### 拆分原则

拆分流程，类似于uml泳道图。在纸上，画业务流程，将可以独立的单一流程，拆成独立的storyboard

### 复用

全局观察ui，将可以复用的提出来
