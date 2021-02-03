# IntelliJ IDEA 錯誤排除



### At least one platform must be selected.
這個是全新安裝 IntelliJ IDEA 之後，安裝了 `Flutter` Plugin 新開專案之後出現的：

![error photo](Upload/Error/IntelliJ_IDEA_flutter_error_at_least_one_platform_must_be_selected.png)

這篇 [Github issues](https://github.com/flutter/flutter-intellij/issues/5230) 有許多人有一樣的問題
看起來問題是出在 `flutter` plugin 更新到 `53` 所導致
解決方式是手動降版本，或等官方修復。
> 52.2.5 下載地址：[https://plugins.jetbrains.com/plugin/9212-flutter/versions/stable/107560](https://plugins.jetbrains.com/plugin/9212-flutter/versions/stable/107560)
