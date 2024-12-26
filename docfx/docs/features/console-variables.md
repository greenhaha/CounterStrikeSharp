---
title: 控制台变量
description: 如何读取和写入控制台变量（ConVars）。
---

# 控制台变量

如何读取和写入控制台变量（ConVars）。

## 查找 ConVar

使用 `ConVar.Find` 静态方法查找对现有 ConVar 的引用（或返回 `null`）。

```csharp
var cheatsCvar = ConVar.Find("sv_cheats");
```

## 操作基本值

读取 ConVar 的值取决于其类型；对于基本值类型的 Cvar（如 float、int、bool），您可以使用 `GetPrimitiveValue` 方法获取该值的 `ref` 引用。

```csharp
cheatsCvar.GetPrimitiveValue<bool>(); // false
```

由于这是作为引用传递的，您可以直接设置该值，它将更改底层值，例如：

```csharp
cheatsCvar.GetPrimitiveValue<bool>() = true; // false

// You can also use the simplified helper:
cheatsCvar.SetValue(true);
```

## 操作字符串

由于需要额外的工作来在 C# 和服务器之间处理字符串，访问字符串的方式有所不同。可以通过 `StringValue` 属性来访问字符串，该属性为您提供了 getter 和 setter 方法。

```csharp
var stringCvar = ConVar.Find("sv_skyname");
Console.WriteLine($"sv_skyname = {stringCvar.StringValue}");
stringCvar.StringValue = "foobar";
```

## 操作原生对象

原生对象也必须以不同的方式处理，因为它们既不是引用也不是字符串。为此，我们可以使用 `GetNativeValue()` 方法。

```csharp
var fogCvar = ConVar.Find("fog_color");
var fogColor = fogCvar.GetNativeValue<Vector>();
Console.WriteLine($"fog_color = {fogColor}");

// You can then manipulate the vector as normal.
fogColor.X = 0.12345;
```
