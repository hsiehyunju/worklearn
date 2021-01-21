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

