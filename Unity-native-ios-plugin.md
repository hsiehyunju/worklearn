# 製作 Unity 的 Native IOS Plugin
雖然 Unity 已經是跨平台的引擎，能夠直接輸出 iOS 平台應用程式，但若遇到以下情況，就必須呼叫原生的 iOS 語言：
- 使用第三方登入平台，但該平台未提供 Unity SDK，但有提供 iOS SDK。
- 串接第三方社群平台的分享功能。
- 串接 Apple 原生程式碼。

範例以 Unity 呼叫 iOS 原生 Alert 來跳出手機顯示訊息。

- [Framework](#framework)
  - [Xcode 專案](#xcode-專案)

## Framework
這一個段落使用 Framework 的形式製作。

### Xcode 專案
開啟 Xcode 選擇 `Create a new Xcode Project`。
![Create a new project in xcode.](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Xcode-create-project-window.png)

選擇 iOS 分頁中的 Framework 樣板。
![Choose a template in xcode](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Xcode-choose-template-for-project.png)

填寫專案名稱，這裡以 `UnityIOSPlugin` 命名專案，選擇 Personal Team 或其他團隊，以 Objective-C 示範，Swift 可能不適用。
![Choose options for project in xcode](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Xcode-choose-options-for-project.png)

儲存路徑依照個人喜好設定，不附圖。

可以新增資料夾用來管理稍後建立的文件。

![Create a new folder in xcode](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Xcode-create-folder.png)

在新增的資料夾上滑鼠右鍵 `New File...` -> `iOS` -> `Cocoa Touch Class` ，定義類別名稱，並繼承 `NSObject` 物件。
![Create cocoa touch class in xcode](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Xcode-create-cocoa-touch-class.png)

當前文件樹狀圖如下：

![Project tree structure](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Xcode-tree-structure-of-folder.png)

`.h` 文件程式碼為：
```objective-c
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN
@interface BridgeController : NSObject
+(void) ShowAlertMessage:(NSString*)title addMsg:(NSString*)message;
@end
NS_ASSUME_NONNULL_END
```

`.m` 文件程式碼為：
```objective-c
#import "BridgeController.h"
#import "UIKit/UIKit.h"

// C Method
void _ShowAlertMessage(const char* title, const char* message)
{
    [BridgeController ShowAlertMessage:[NSString stringWithUTF8String:title] addMsg:[NSString stringWithUTF8String:message]];
}

@implementation BridgeController
+(void) ShowAlertMessage:(NSString*)title addMsg(NSString*)message {
    // Print Log
    NSLog(@"輸入的 title : %@ / message : %@", title, message);
    
    // Alert
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
    
    UIAlertAction *defaultAction = [UIAlertAction actionWithTitle:@"OK" style:UIAlertStyleDefault handler:^(UIAlertAction *action){}];
    
    [alert addAction:defaultAction];
}
@end
```
