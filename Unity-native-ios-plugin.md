# 製作 Unity 的 Native IOS Plugin
雖然 Unity 已經是跨平台的引擎，能夠直接輸出 iOS 平台應用程式，但若遇到以下情況，就必須呼叫原生的 iOS 語言：
- 使用第三方登入平台，該平台未提供 Unity SDK，但有提供 iOS SDK。
- 串接第三方社群平台的分享功能。
- 串接 Apple 原生程式碼。

範例以 Unity 呼叫 iOS 原生 Alert 來跳出手機顯示訊息。

- [Framework](#framework)
  - [Xcode Project](#xcode-project)
  - [Unity Project](#unity-project)
  - [Build](#build-and-test)
- [Other](#Other)
  - [模擬器測試 Test on iOS Simulator](#test-on-ios-simulator)

## Framework
這一個段落使用 Framework 的形式製作。

### Xcode Project
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

### Unity Project
Unity 新增新專案，由於測試用所以我僅選擇 2D Templete，專案名稱自行命名。
![Unity-create-a-project](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-create-a-project.png)

先建立這樣子的資料夾結構：

![Unity-Create-Folder](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-create-folder.png)

建立串接腳本，我自己命名為 `BridgeController.cs` ，內容如下：
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// DllImport 要使用
using System.Runtime.InteropServices;

public class BridgeController : MonoBehaviour
{
    // 與 C 串接的方法，需與 Xcode Objective-C 中撰寫的方法名一樣
    [DllImport("__Internal")]
    private static extern void _ShowAlertMessage(string title, string message);
    
    // Unity Call method.
    public static void ShowAlertMessage(string title, string message)
    {
        _ShowAlertMessage(title, message);
    }
}
```
![Unity-Create-scripts](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-bridgecontroller-code.png)

建立 Unity 互動腳本，我自己命名為 `Alert.cs` ，內容如下：
```csharp
using UnityEngine;
using UnityEngine.UI;

public class Alert : MonoBehaviour
{
    
    // 對應欄位輸入 UI
    public InputField titleInputField, messageInputField;
    
    // 按鈕按下呼叫的方法
    public void ButtonClick()
    {
        // 判斷欄位有輸入文字
        if (titleInputField.text != "" && messageInputField.text != "")
        {
            // 判斷平台是 iOS
            if (Application.platform == RuntimePlatform.IPhonePlayer
                BridgeController.ShowAlertMessage(titleInputField.text, messageInputField.text);
            else
                Debug.LogError("Not Support");
        {
        else
            Debug.LogError("Fieldl is empty");
    }
}
```
![Unity-Create-scripts](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-alert-code.png)

建立自己的對應 UI：

![Unity-Create-ui](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-ui-design.png)

將 `Alert.cs` 腳本放到想放的物件上，並拉上對應元件：

![Unity-button-settings](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-button-ui-settings.png)

回到 Xcode 專案找到 `.h` 和 `.m` 兩隻檔案，右鍵 `Show in Finder`

![Show in finder](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-xcodefile-show-in-finder.png)

將文件複製貼到 Unity 中的 `Plugins` 的 `IOS` 下，如圖：

![copy to unity](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-xcodefile-copy-to-unity.png)

確認兩個文件 Include Platforms 中的 iOS 是勾選狀態：

![check platform](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-check-platform.png)

切換平台，並設置好 `Player Settings` 內相關設定

![Unity-switch-platform](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Unity-switch-platform.png)

Build 輸出 Xcode 專案。

### Build and Test
找到輸出的資料夾，開啟 `Unity-iPhone.xcodeproj`。
![Open-xcode-project](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Build-open-xcodeproj.png)

找到資料夾結構中的 `BridgeController.m`：

![find-bridgecontroller](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Build-find-file.png)

在程式碼的靜態方法中引用 Unity 函示庫
```objective-c
// 引用 Unity 函式庫
[UnityGetGLViewController() persentViewController:alert animated:YES completion:nil];
```
![call-unity-framework-method](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Build-add-code.png)

確保 Project Signing & Capabilities 已正確設置：
![xcoe-signing](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Build-xcode-signing.png)

Build 實機測試：

![build-iphone](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Build-iphone.png)








---
# 其他

### Test on iOS simulator
在 Unity 專案中，`Player Settings` -> `Other Settings` -> `Configuration` -> `Target SDK` = `Simulator SDK`。
![Test on simulator](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Other-test-on-simulator.png)

若出現錯誤，請修改 `Graphics API` 為下圖：

![fix-graphics-api](https://github.com/hsiehyunju/worklearn/blob/main/Upload/UnityNativeIOSPlugin/Other-auto-graphics-api.png)
