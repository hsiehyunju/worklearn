# Dart 學習歷程
已有 C# 基礎，因學習 Flutter 就順便寫起來方便以後讀閱。

# 目錄
- [字串插值](#字串插值)

---

### 字串插值

|  | Dart | C# |
|---------|---------|----------|
| 定義符號 | $variableName、${expression}| $"" +{}|

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
