# Dart 學習歷程
已有 C# 基礎，因學習 Flutter 就順便寫起來方便以後讀閱。

# 目錄
- [常數](#常數-constant)
- [字串插值](#字串插值)
- [函式](#函式-function)
  - [引數](#引數-arguments)
    - [可選引數](#可選引數-optional)
- [集合](#集合-collections)


---
### 常數 Constant
||Dart|C#|
|---------|---------|----------|
|編譯常量 compile-time constants|`const`|`const`|
|執行時間常量 run-time constants|`final`|`readonly`|

### 字串插值

|  | Dart | C# |
|---------|---------|----------|
| 定義符號 | $variableName、${expression}| $"{}"|

Dart 
```dart
var pi = 3.14;
print('pi is $pi');
print('tau is ${2 * pi}');
```

C#
```csharp
var pi = 3.14;
Console.WriteLine($"pi is {pi}");
Console.WriteLine($"tau is {2 * pi});
```

---
### 函式 Function
#### 引數 Arguments
##### 可選引數 Optional
Dart、C# 在宣告可選引數時，共通的特點就是都**必須放在非可選引數後面**。
||Dart|C#|
|---------|---------|----------|
|限制|{}或[]包覆|必須有初始值|

Dart `{}`、`[]` 比較
|Dart|`{}`|`[]`|
|---------|---------|----------|
|初始值|可不用|可不用|
|變數名稱|use|unuse|
|必須按順序傳遞|NO|YES|

```dart
PrintPerson(String name, {int age, String gender}) {
  // age和gender為可選參數，可以不傳
}

PrintPerson2(String name ,[int age, String gender]) { 
  //age和gender為可選參數，不使用變數名稱，依照順序傳值
  //可選參數需定義於固定參數後面
}
```

---
### 集合 Collections

||Dart|C#|
|---------|---------|----------|
|清單|`List<T>`|`List<T>`|
|字典|`map<TKey, TValue>`|`Dictionary<TKey, TValue>`|
|先進先出 FIFO|`Queue`|`Queue`|
|後進先出 LIFO|-|`Stack`|
|不重複集合|`Set`|-(可使用LINQ)|
|不重複字典|-|`SortedList<TKey,TValue>`|
