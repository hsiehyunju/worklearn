# 製作 Unity 的 Native Android Plugin
[前言請參考我](Unity-native-ios-plugin.md)
> Android Studio 版本不同可能按鈕位置不同，請尋找一下。

## 目錄
- [Library](#library)
  - [Android Studio](#android-studio-project)
  - [Unity](#unity-project)
  - [Callback](#unity-callback)
  
---

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

找到以下路徑 UnityPlugin/src/main/java/`專案名稱` 新建 Java Class：

![Create java class](Upload/UnityNativeAndroidPlugin/AndroidStudio_create_java_class.png)

新的 Java Class 命名為 `BridgeController`，可以自行命名：

![Create java class](Upload/UnityNativeAndroidPlugin/AndroidStudio_create_java_name.png)

新增一個靜態的方法，讓 Unity 傳遞文字給這個方法，該方法回傳 bool 值：
```java
public static boolean PrintLog(String msg)
{
    Log.i("Unity", msg);
    return true;
}
```

利用 Gradle 視窗 build ：

![Gradle build](Upload/UnityNativeAndroidPlugin/AndroidStudio_build_first.png)

---

### Unity Project
新增一個測試用專案，並選好儲存位置，創建如下資料夾結構：

![Unity folder](Upload/UnityNativeAndroidPlugin/Unity_folder.png)

打開剛才 Android Studio 專案，找到剛才打包的 `aar` 文件，路徑如下：
> 專案資料夾\Library名稱\build\outputs\aar

將 `Library名稱-release.aar` 複製到 Unity : Assets\Plugins\Android：

![Unity import aar](Upload/UnityNativeAndroidPlugin/Unity_import_aar.png)

新增 `BridgeController.cs` 腳本。作為與 Android Java 溝通的程式：
```csharp
using UnityEngine;

public class BridgeController : MonoBehaviour
{
    public static void PrintLog()
    {
        // Java Library package name + class name.
        using (AndroidJavaClass jc = new AndroidJavaClass("com.test.unityplugin.BridgeController"))
        {
            // 呼叫靜態方法
            if (jc.CallStatic<bool>("PrintLog", "Unity 送出的 Log"))
            {
                Debug.Log("Unity 收到方法回傳");
            }
        }
    }
}
```

![Unity bridgecontroller.cs](Upload/UnityNativeAndroidPlugin/Unity_bridgecontroller_cs.png)

接著新增 `Test.cs` 腳本，用來呼叫 BridgeController，並掛在 Camera 物件上：

```csharp
using UnityEngine;

public class Test : MonoBehaviour
{

    void Start()
    {
        BridgeController.PrintLog();
    }
}
```

Unity Build APK，並記得設定相關環境。

![Unity build](Upload/UnityNativeAndroidPlugin/Unity_build.png)

開啟 Android Studio 切換到 `Logcat` 視窗，標籤可以打上 Unity：

![test](Upload/UnityNativeAndroidPlugin/Unity_test_androidstudio.png)

安裝 APK 到模擬器上並打開，可以觀察 `Logcat` 視窗：

![test](Upload/UnityNativeAndroidPlugin/Unity_test_logcat.png)

---

### Unity Callback
這邊比較特別的是，順便紀錄在 Android Studio 引用 Unity 的 jar

> 開啟以下路徑：C:\Program Files\Unity\Hub\Editor\2018.4.30f1\Editor\Data\PlaybackEngines\AndroidPlayer\Variations\mono\Release\Classes
> Unity 版本不同，位置可能不同，需搜尋一下

將 `classes.jar` 複製到 Android Studio Library lib 資料夾下：

![copy paste](Upload/UnityNativeAndroidPlugin/UnityCallback_copypaste_jar.png)

複製過去

![copy jar](Upload/UnityNativeAndroidPlugin/UnityCallback_copy_unity_jar.png)

對 `classes.jar` 右鍵 `Add As Library...` 選擇 Library

![Create library](Upload/UnityNativeAndroidPlugin/UnityCallback_create_library.png)

由於這裡引入 `classes.jar` 後，輸出到 Unity Build 之後會重複並錯誤，所以要在 Gradle 做設定，讓 Gradle 編譯可以過，但並不打包
開啟 Library 下的 Gradle ，修改後記得 `Sync Now`：

![Unity folder](Upload/UnityNativeAndroidPlugin/UnityCallback_create_library.png)

開啟 `BridgeController.java` 修改成以下：
```java
package com.test.unityplugin;

import android.app.AlertDialog;
import android.content.DialogInterface;
import com.unity3d.player.UnityPlayer;

public class BridgeController {

    public static void ShowAlertDialog(String title, String msg)
    {

        // 新增 OK 按鈕的對應 Action
        DialogInterface.OnClickListener alertOKBtnClick = new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {

                // 呼叫 Unity Send Message ("物件名稱","方法名稱","參數")
                UnityPlayer.UnitySendMessage("UnityCallback", "AlertOK", "");

            }
        };

        // 新增 Cancel 按鈕的對應 Action
        DialogInterface.OnClickListener alertCancelBtnClick = new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {

                // 呼叫 Unity Send Message ("物件名稱","方法名稱","參數")
                UnityPlayer.UnitySendMessage("UnityCallback", "AlertCancel", "");

            }
        };

        // Java AlertDialog
        new AlertDialog.Builder(UnityPlayer.currentActivity)
                .setTitle(title)
                .setMessage(msg)
                .setPositiveButton("OK", alertOKBtnClick)
                .setNegativeButton("Cancel", alertCancelBtnClick)
                .show();
    }
}
```

利用 Gradle 視窗的 `Clear` 先刪除舊的檔案，再用 `Build` 重新編譯，編譯後重新覆蓋 Unity 內的檔案。

建立一個空物件 `UnityCallback`，並增加同名腳本，內容如下：
```csharp
using UnityEngine;
using UnityEngine.UI;

public class UnityCallback : MonoBehaviour
{

    public InputField title, message;

    public void Alert()
    {
        BridgeController.ShowAlertMessage(title.text, message.text);
    }


    public void AlertOK()
    {
        Debug.Log("使用者點選 OK");
    }

    public void AlertCancel()
    {
        Debug.LogError("使用者 Cancel");
    }
}
```

原本的 Test.cs 可以刪除，BirdgeController 內容改成以下：
```csharp
using UnityEngine;

public class BridgeController : MonoBehaviour
{
    public static void ShowAlertMessage(string title, string msg)
    {
        // Java Library package name + class name.
        using (AndroidJavaClass jc = new AndroidJavaClass("com.test.unityplugin.BridgeController"))
        {
            // 呼叫靜態方法
            jc.CallStatic("ShowAlertDialog", title, msg);
        }
    }
}
```

建立 UI 如下：

![Unity UI](Upload/UnityNativeAndroidPlugin/UnityCallback_UI.png)

Unity 重新輸出 APK 測試：


![Unity UI](Upload/UnityNativeAndroidPlugin/UnityCallback_simulate_test.png)


![Unity UI](Upload/UnityNativeAndroidPlugin/UnityCallback_result.png)
