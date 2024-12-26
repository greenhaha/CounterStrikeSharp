---
title: 游戏事件
description: 如何监听 Source 1 风格的游戏事件。
---

# Game Events

如何监听 Source 1 风格的游戏事件。
## 添加一个事件监听器

### 自动注册

CounterStrikeSharp 将自动注册在 `BasePlugin` 类上使用 `GameEventHandler` 属性标记的事件监听器。这些监听器会在热重载时自动注册/注销。

> [!NOTE]
> 第一个参数类型必须是`GameEvent` 类的子类。名称会根据 [游戏事件列表](https://cs2.poggu.me/dumped-data/game-events) 自动生成。

```csharp
[GameEventHandler]
public HookResult OnPlayerConnect(EventPlayerConnect @event, GameEventInfo info)
{
    // Userid will give you a reference to a CCSPlayerController class.
    // Before accessing any of its fields, you must first check if the Userid
    // handle is actually valid, otherwise you may run into runtime exceptions.
    // See the documentation section on Referencing Players for details.
    if (@event.Userid.IsValid) {
        Logger.LogInformation("Player {Name} has connected!", @event.Userid.PlayerName);
    }

    return HookResult.Continue;
}
```

### On Load

在 `OnLoad` 方法中（或在任何可以访问插件实例的地方）也可以绑定事件监听器。

```csharp
public override void Load(bool hotReload)
{
    RegisterEventHandler<EventRoundStart>((@event, info) =>
    {
        Logger.LogInformation("Round has started with time limit of {Timelimit}", @event.Timelimit);

        return HookResult.Continue;
    });
}
```

## 访问事件参数

`GameEvent` 的特定子类将根据事件定义提供强类型的参数。例如，`event.Timelimit` 将是一个 `long` 值，`event.UserId` 将是一个 `CCSPlayerController`，等等。

这些事件属性是可变的，因此您可以像平常一样更新它们，它们将在事件实例中更新。


> [!CAUTION]
> `GameEvent` instances and their properties will cease to exist after the event listener function is called, which means that you will encounter errors when accessing properties in timers and functions like `Server.NextFrame()`. You should store the value of properties in variables before calling functions like `Server.NextFrame()` so you can read the data safely.

## Preventing Broadcast

## 阻止广播

您可以通过修改 `bool info.DontBroadcast` 属性来阻止游戏事件广播到客户端。 等等

## 取消事件

在预事件钩子中，您可以通过返回 `HookResult.Handled` 或 `HookResult.Stop` 来阻止事件继续传递到其他插件。
