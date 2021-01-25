# 製作 Unity 的 Native Android Plugin
[前言請參考我](Unity-native-ios-plugin.md)
> Android Studio 版本不同可能按鈕位置不同，請尋找一下。

## 目錄
- [Library](#library)

## Library
這一個段落使用 Library 的形式製作。

### Android Studio Project
開啟 Android Studio 選擇 `Create New Project`。

![Create new project in android studio](Upload/UnityNativeAndroidPlugin/AndroidStudio_create_project.png)

練習用，這裡選擇 `No Activity` 視需求選擇

![Select a project template](Upload/UnityNativeAndroidPlugin/AndroidStudio_select_project_template.png)

設定專案，選擇要儲存的地方，這裡的 `Minimim SDK` 要跟 Unity 的一樣。

![Configure your project](Upload/UnityNativeAndroidPlugin/AndroidStudio_configure_project.png)   

在 Android Studio 中選擇 `File` > `New` > `New Module`。

![Create a new module](Upload/UnityNativeAndroidPlugin/AndroidStudio_create_new_module.png)

選擇 `Android Library` 的類型

![Select a module type](Upload/UnityNativeAndroidPlugin/AndroidStudio_select_module_type.png)

設定 Android Library

![Library setting](Upload/UnityNativeAndroidPlugin/AndroidStudio_library_setting.png)

檢查 `Project` 中是否有 Library 的資料夾
![Check Folder](Upload/UnityNativeAndroidPlugin/AndroidStudio_project_have_library_folder.png)

打開 Library 資料夾下的 `build.gradle` 刪減剩下圖，並按同步按鈕：
![Modify build gradle](Upload/UnityNativeAndroidPlugin/AndroidStudio_modify_build_gradle.png)
