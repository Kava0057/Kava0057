- 👋 Hi, I’m @Kava0057
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Kava0057/Kava0057 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
//
//  AppDelegate.m
//  iOS8
//
//  Created by Jeremy Cope on 5/13/14.
//  Copyright (c) 2014 Emma Technologies, L.L.C. All rights reserved.
//

#import "AppDelegate.h"
#import "DemoNC.h"
#import "HomeVC.h"
@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
    
    //Launch the Demo Navigation
    //This creates the Demo Apps and the Home View for presentation
    DemoNC* navigation = [[DemoNC alloc] init];
    
    [self.window setRootViewController:navigation];
    
    [self.window makeKeyAndVisible];
    return YES;
}

@end
//
//  AppDelegate.h
//  iOS8
//
//  Created by Jeremy Cope on 5/13/14.
//  Copyright (c) 2014 Emma Technologies, L.L.C. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (strong, nonatomic) UIWindow *window;

@end
//
//  BackgroundVC.m
//  iOS8
//
//  Created by Jeremy Cope on 5/13/14.
//  Copyright (c) 2014 Emma Technologies, L.L.C. All rights reserved.
//

#import "BackgroundVC.h"

@interface BackgroundVC ()
@property BOOL activeBackground;
@property UIImageView* background;
@property NSArray* imageArray;
@property NSInteger imageIndex;
@end

@implementation BackgroundVC

- (id)init{
    if (self = [super init]) {
        self.view.backgroundColor = [UIColor whiteColor];
        _background = [[UIImageView alloc] initWithFrame:self.view.frame];
        [_background setImage:[UIImage imageNamed:@"Background.png"]];
        [_background setAlpha:0.0];
        [self.view addSubview:_background];
        
        _imageIndex = 0;
        _activeBackground = NO;
        _imageArray = @[[UIImage imageNamed:@"Background1.png"],
                        [UIImage imageNamed:@"Background2.png"],
                        [UIImage imageNamed:@"Background3.png"],
                        [UIImage imageNamed:@"Background4.png"],
                        [UIImage imageNamed:@"Background5.png"],
                        [UIImage imageNamed:@"Background6.png"]];
    }
    return self;
}
//Fade in the background
-(void)setupWithCompletion:(void (^)(void))completion{
    [UIView animateWithDuration:2.7 animations:^{
        [_background setAlpha:1.0];
    } completion:^(BOOL finished) {
        completion();
    }];
}
-(void)start{
    _activeBackground = YES;
    //Start Timer
    [self dissolveBackground];
}
-(void)stop{
    _activeBackground = NO;
}
-(void)dissolveBackground{
    //Cross Dissolve into the Next Background Image
    [UIView transitionWithView:self.view
                      duration:2.7f
                       options:UIViewAnimationOptionTransitionCrossDissolve
                    animations:^{
                        _background.image = _imageArray[(_imageIndex++)%[_imageArray count]];
                    } completion:^(BOOL finished) {
                        if(_activeBackground){
                            [self dissolveBackground];
                        }
                    }];
    
}
@end
//
//  BackgroundVC.h
//  iOS8
//
//  Created by Jeremy Cope on 5/13/14.
//  Copyright (c) 2014 Emma Technologies, L.L.C. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface BackgroundVC : UIViewController
-(void)setupWithCompletion:(void (^)(void))completion;
-(void)start;
-(void)stop;

@end


