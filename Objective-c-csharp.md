# Objective-C 學習歷程
已有 C# 基礎，因工作需求學習 Objective-C 而做的兩種語言比較，可以讓自己以後快速翻閱。

參考原文連結 : [iOS開發60分鐘入門](https://github.com/qinjx/30min_guides/blob/master/ios.md) (Java對比)

# 目錄
- [字串](#字串)
- [方法](#方法)
    - [呼叫方法-不傳遞參數](#呼叫方法不傳遞參數)
    - [呼叫方法-傳遞參數](#呼叫方法傳遞參數)
- [類別](#類別)
    - [定義與實作](#類別定義與實作)
    - [靜態與非靜態方法](#靜態方法與非靜態方法的宣告)
- [繼承](#繼承)
- [協議](#協議)
    - [定義](#協議的定義)
    - [繼承](#協議的繼承)
    - [Optional](#可選方法)
    - [實作](#實作)
- [Category](#Category)


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

### 類別定義與實作
Objective-C 中的類別分為兩個部分，分別是 `Interface(定義)` 文件副檔名為 `.h` 和 `Implementation(實作)` 文件副檔名為 `.m`
> Interface 的概念最接近的是C、C++裡的 Header files 標頭檔，在 Objective-C 中它與 Implementation 是成雙成對出現，前者用意是公開(public)方法的實作，後者定義私有（private）變數及方法。

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

@implementation User {
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
    public static void StaticMethod() {};
    // 公開的非靜態方法，包含實作。
    public void NonStaticMethod() {};
}
```

# 繼承
Objective-C 的繼承與 C# 中的繼承方式一樣。

**Objective-C**
```objective-c
@interface TestClass : NSObject
{
}
@end
```

**C#**
```csharp
public class TestClass : BaseClass
{
}
```

# 協議
Objective-C 中的`協議(Protocol)`，等於 C# 中的`介面(Interface)`。

### 協議的定義
Objective-C 中定義協議使用`@protocol`關鍵字。

```objective-c 
@protocol Printable
    -(void)print:(NSString)str;
@end
```

C# 中定義介面使用 `interface` 關鍵字，並於命名中首字採用 `I`。
```csharp
public interface IPrintable
{
    void Print(string str);
}
```

### 協議的繼承
在 Objective-C 中協議能夠繼承其他協議，C# 也同樣可以。

Objective-C
```objective-c 
@interface InterfaceA
@end

@interface InterfaceB <InterfaceA>
@end
```

C#
```csharp
public interface InterfaceA {}
public interface InterfaceB : InterfaceA {}
```
### 可選方法
在 Objective-C 中，協議可以宣告可選方法，使用 `optional` 關鍵字讓方法能被繼承的類別選擇是否實作。

Objective-C
```objective-c
@protocol Printable
@optional
	-(void)print:(NSString)str;
@end
```

在 C# 中，`介面(interface)` 所定義的方法，都是必須被實作的，若有相關需求請使用 `抽象類別(abstract)` 。
> 對於 C# 中 `interface`、`abstract`、`virtual` 的使用方式，可參考[此連結](https://dotblogs.com.tw/enet/2017/01/04/122935)

### 實作
Objective-C : TestClass 繼承 NSObject 並且實作 Protocol1、Protocol2 兩個協議。
```objective-c
@interface TestClass : NSObject <Protocol1, Protocol2>
@end
```
C# : TestClass 繼承 BaseClass 並且實作 IProtocol1、IProtocol2 兩個介面。
```csharp
public class TestClass : BaseClass, IProtocol1, IProtocol2
{
	;
}
```

# Category
不太確定這個怎麼翻譯，不過用途就是對現有的類別進行額外方法的擴充，而不用動到原始文件。
> 範例定義為：增加一個方法用於讓字串輸出時，在後面增加123 

> ex: 王大明 > call method > out : 王大明123

> 額外文件以 `extension` 命名

1. **Objective-C 規則**
```objective-c
@interface 原Class名稱 (Category Name)
// function 宣告
@end
 
@implementation 原Class名稱 (Category Name)
// function 實做
@end
```

2. **Objective-C extension.h**
```objective-c
@interface NSString (NSStringCategory)
+(NSString*) Add123Number: (NSString*) str;
@end
```

3. **Objective-C extension.m**
```objective-c
@implementation NSString (NSStringCategory)
{
    return [NSString stringWithFormate:@"%@%@", str, @"123"];
}
@end
```

4. **Objective-C 使用**
```objective-c

// 一定要 import
#import "extension.h"

// 假設在方法中使用
-(void) Print {
    NSString* output = [NSString Add123Number:@"王大明"];
    NSLog(@"%@", output);
}
```

5. **C# extension.cs**
```csharp
using System;

namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static string Add123Number(this String str)
        {
            return $"{str}123";
        }
    }
}
```

6. **C# extension 使用**
```csharp
// 一定要 using
using ExtensionMethods;

// 假設在方法中使用
public void Print()
{
    string name = "王大明";
    string output = name.Add123Number();
    System.Console.WriteLine(output);
}
```
