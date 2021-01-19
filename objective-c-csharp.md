# Objective-C 學習歷程
已有 C# 基礎，因工作需求學習 Objective-C 而做的兩種語言比較，可以讓自己以後快速翻閱。

# 目錄
- [字串](#字串)
- [方法](#方法)
    - [呼叫方法-不傳遞參數](#呼叫方法不傳遞參數)
    - [呼叫方法-傳遞參數](#呼叫方法傳遞參數)
- [類別](#類別)
    - [定義與實現](#類別定義與實現)
    - [靜態與非靜態方法](#靜態方法與非靜態方法的宣告)


# 字串
在 Objective-C 內字串是由`@(音at)`開頭並由雙引號包覆：
```objective-c
name = @"王大明";
```
C# 中的字串
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
呼叫 `data` 物件下的 `SetName` 方法，並傳遞**一個**字串參數時：

Objective-C
```objective-c
[data SetName:@"王大明"];
```

C#
```csharp
data.SetName("王大明");
```

呼叫 `data` 物件下的 `SetName` 方法，並傳遞**多個**字串參數時：

Objective-C-
```objective-c
[data SetName:@"大明" lastName:@"王"];
```

C#
```csharp
data.SetName("大明","王");
```

# 類別

### 類別定義與實現
Objective-C 中的類別分為兩個部分，分別是 `Interface(定義)` 文件副檔名為 `.h` 和 `Implementation(實現)` 文件副檔名為 `.m`
> Interface 的概念最接近的是C、C++裡的 Header files 標頭檔，在 Objective-C 中它與 Implementation 是成雙成對出現，用意是公開(public)方法的實現，以及定義私有（private）變數及方法。

新增一個 User 類別內有兩個字串變數、一個無回傳並有參數的 `SetName` 方法、一個有字串回傳的 `GetName` 方法。

1. **Objective-C Interface User.h**
```objective-c
@interface User : NSObject

-(void) SetName: (NSString*)_firstName lastName:(NSString*)_lastname;
-(NSString*) GetName ;

@end
```

2. **Objective-C Implementation User.m**
```objective-c
#import "User.h"

@implementation Test {
    NSString *lastName;
    NSString *firstName;
}

-(void) SetName: (NSString*)_firstName lastName:(NSString*)_lastname
{
    firstName = _firstName;
    lastName = _lastname;
}

-(NSString*) GetName {
    return [NSString stringWithFormat:@"%@%@", firstName, lastName];
}

@end
```

3. **C# User.cs**
```csharp
public class User 
{
    private string lastName;
    private string firstName;

    public void SetName(string _lastName, string _firstName)
    {
        lastName = _lastName;
        firstName = _firstName;
    }    

    public string GetName()
    {
        return String.Format("{0}{1}", firstName, lastName);
        // return $"{firstName}{lastName}";
    }
}
```

### 靜態方法與非靜態方法的宣告

1. **Objective-C TestStaticMethod.h**
```objective-c 
@interface TestStaticMethod
+(void) StaticMethod;    // 宣告靜態公開方法，不包含實作，實作請在.m檔中完成。
-(void) NonStaticMethod; // 宣告非靜態公開方法，不包含實作，實作請在.m檔中完成。
@end
```

2. **C# TestStaticMethod.cs**
```csharp
public class TestStaticMethod 
{
    // 公開的靜態方法，包含實作。
    public static void StaticMethod();
    // 公開的非靜態方法，包含實作。
    public void NonStaticMethod();
}
```
