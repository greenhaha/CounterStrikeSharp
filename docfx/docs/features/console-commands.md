---
title: 控制台命令
description: 如何添加一个新的控制台指令
---

# 控制台命令

如何添加一个新的控制台指令

## 添加一个新的控制台指令

### 自动注册

CounterStrikeSharp 将自动注册在 `BasePlugin` 类上使用 `ConsoleCommand` 属性标记的控制台命令。这些命令会在热重载时自动注册/注销。

```csharp
[ConsoleCommand("custom_command", "This is an example command description")]
public void OnCommand(CCSPlayerController? player, CommandInfo command)
{
    if (player == null) {
        Console.WriteLine("Command has been called by the server.");
        return;
    }

    Console.WriteLine("Custom command called.");
}
```

### On Load

在 `OnLoad` 方法中（或在任何可以访问插件实例的地方）也可以绑定命令。

```csharp
public override void Load(bool hotReload)
{
    AddCommand("on_load_command", "A command is registered during OnLoad", (player, info) =>
    {
        if (player == null) return;
        Console.WriteLine($"Custom command called.");
    });
}
```

## 访问命令参数

`CommandInfo` 类提供对调用命令参数的访问。第一个参数始终是被调用的命令。带引号的值将以未带引号的形式返回，例如：

```csharp
[ConsoleCommand("custom_command", "This is an example command description")]
public void OnCommand(CCSPlayerController? player, CommandInfo command)
{
    Console.Write($@"
Arg Count: {command.ArgCount}
Arg String: {command.ArgString}
Command String: {command.GetCommandString}
First Argument: {command.ArgByIndex(0)}
Second Argument: {command.ArgByIndex(1)}");
}
```

```shell
> custom_command "Test Quoted" 5 13

# Output
Arg Count: 4
Arg String: "Test Quoted" 5 13
Command String: custom_command "Test Quoted" 5 13
First Argument: custom_command
Second Argument: Test Quoted
```

## 辅助属性

CounterStrikeSharp 提供了 `CommandHelper` 属性，用于命令方法（函数回调），以简化检查参数数量的过程，并限制命令只能由服务器控制台或玩家（或两者）执行。

```csharp
[ConsoleCommand("freeze", "Freezes a client.")]
[CommandHelper(minArgs: 1, usage: "[target]", whoCanExecute: CommandUsage.CLIENT_AND_SERVER)]
public void OnFreezeCommand(CCSPlayerController? caller, CommandInfo command)
{
    ...
}
```

如果客户端在没有 `[target]` 参数的情况下尝试执行命令，它将在聊天中向他们打印一条消息：


```shell
[CSS] Expected usage: "!freeze [target]".
```

如果命令被错误的用户执行，它将向他们打印一条消息：

```shell
[CSS] This command can only be executed by clients.
```

有效的 `CommandUsage` 值：

```csharp
public enum CommandUsage
{
    CLIENT_AND_SERVER = 0,
    CLIENT_ONLY,
    SERVER_ONLY
}
```
