---
title: 开始教程
description: 如何开始安装和使用 CounterStrikeSharp。
---

# 开始教程

在本指南中，您将学习如何将 CounterStrikeSharp 安装到原版的 Counter-Strike 2 服务器上。 `CounterStrikeSharp` 使用 `Metamod:Source` 作为其与游戏服务器通信的主要方式，因此需要安装这两个框架。

如果您更喜欢视频教程，可以点击此处观看 <a href="https://www.youtube.com/watch?v=FlsKzStHJuY" target="_blank">YouTube 视频</a> 

## 先决条件
- <a href="https://www.sourcemm.net/downloads.php/?branch=master" target="_blank">Metamod: Source 2.X Dev Build</a>
- <a href="https://github.com/roflmuffin/CounterStrikeSharp/releases" target="_blank">CounterStrikeSharp With Runtime</a>

## 安装 Metamod

1. 解压 Metamod，并将 `/addons/` 目录复制到 `/game/csgo/` 中。
2. 在 `/game/csgo/` 目录下找到 `gameinfo.gi` 文件。
3. 在 `Game_LowViolence    csgo_lv` 下方新建一行，并添加 `Game    csgo/addons/metamod`。
4. 重启您的游戏服务器。

您的 `gameinfo.gi` 文件应如下图所示：[示例图片](../../images/gameinfogi-example.png)。在服务器控制台中输入 `meta list`，查看 Metamod 是否已加载。

## 安装 CounterStrikeSharp

1. 解压 CounterStrikeSharp，并将 `/addons/` 目录复制到 `/game/csgo/` 中
2. 重启游戏服务器。

在控制台中运行命令 `meta list`，您应会看到已加载 1 个插件 🎉

```shell
meta list
Listing 1 plugin:
  [01] CounterStrikeSharp (0.1.0) by Roflmuffin
```

> [!注意]
> 对于 Windows 服务器，必须安装 Visual Studio Redistributables，否则 CounterStrikeSharp 将无法正常工作。

## 升级 CounterStrikeSharp


要升级 CounterStrikeSharp，只需下载最新版本并将其复制到服务器中，步骤与初次安装相同。

CounterStrikeSharp 设计时不会覆盖您的配置文件，因此可以安全地更新。由于已经安装了 CounterStrikeSharp，可以下载非 `with-runtime` 版本，但需要确保您的 .NET 运行时是最新的。
 

## 故障排除

- 如果是首次安装，**必须**下载 `with-runtime` 版本。该版本包含 .NET 运行时，这是插件运行所必需的。
- 根据您的操作系统，您可能还需要使用软件包管理器安装 `libicu` / `icu-libs` / `libicu-dev` 以支持 .NET 运行。
- 如果在控制台中输入 `meta list` 时出现 `Unknown Command`，请检查文件夹是否正确复制，`gameinfo.gi` 文件是否已正确修改。

文件结构应如下所示：

```shell
<server_path>/game/csgo/addons > tree -L 2
addons
├── counterstrikesharp
│   ├── api
│   ├── bin
│   ├── dotnet
│   ├── plugins
│   └── gamedata
│
├── metamod
│   ├── bin
│   ├── counterstrikesharp.vdf
│   ├── metaplugins.ini
│   └── README.txt
├── metamod.vdf
└── metamod_x64.vdf
```
