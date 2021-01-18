# Objective-C 學習歷程
已有 C# 基礎，因工作需求學習 Objective-C 而做的兩種語言比較，可以讓自己以後快速翻閱。

# 目錄
- [字串](#字串)
- [方法](#方法)
    - [呼叫方法-不傳遞參數](#呼叫方法不傳遞參數)


# 字串
在 Objective-C 內字串是由`@(音at)`開頭並由雙引號包覆：
```objective-c
name = @"王大明";
```
C#　中的字串
```csharp
name = "王大明";
```

# 方法

### 呼叫方法(不傳遞參數)
呼叫 `data` 物件下的 `GetName` 方法時：

Objective-C
```objective-c
[data GetName];
```

C#
```csharp
data.GetName();
```

### 呼叫方法(傳遞參數)
呼叫 `data` 物件下的 `SetName` 方法，並傳遞一個字串參數時：

Objective-C
```objective-c
[data SetName:@"王大明"];
```

C#
```csharp
data.SetName("王大明");
```

呼叫 `data` 物件下的 `SetName` 方法，並傳遞多個字串參數時：

Objective-C-
```objective-c
[data SetName:@"大明" lastName:@"王"];
```

C#
```csharp
data.SetName("大明","王");
```

