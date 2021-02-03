# Dart 學習歷程
已有 C# 基礎，因學習 Flutter 就順便寫起來方便以後讀閱。

# 目錄
- [常數](#常數-constant)
- [字串插值](#字串插值)
- [集合](#集合-collections)


---
### 常數 Constant
Keyword
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
### 集合 Collections

||Dart|C#|
|---------|---------|----------|
|清單|`List<T>`|`List<T>`|
|字典|`map<TKey, TValue>`|`Dictionary<TKey, TValue>`|
|先進先出 FIFO|`Queue`|`Queue`|
|後進先出 LIFO|-|`Stack`|
|不重複集合|`Set`|-(可使用LINQ)|
|不重複字典|-|`SortedList<TKey,TValue>`|
