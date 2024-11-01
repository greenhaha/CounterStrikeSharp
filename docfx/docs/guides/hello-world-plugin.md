---
title: Hello World Plugin
description: 如何为 CounterStrikeSharp 编写第一个插件。
---

# Hello World Plugin

如何为 CounterStrikeSharp 编写第一个插件

## 创建新项目

首先，确保您在计算机上安装了适合您平台的 .NET 8.0 SDK。您可以在<a href="https://dotnet.microsoft.com/en-us/download/dotnet/8.0" target="_blank"> 官方 Microsoft 下载页面</a>找到最新下载的链接。.

### 创建类库
所有 CounterStrikeSharp 插件都作为构建好的 .dll 类库二进制文件安装在服务器上，因此我们将通过使用内置的 dotnet SDK 工具来创建一个新的类库。

```shell
dotnet new classlib --name HelloWorldPlugin
```

使用您的 IDE（Visual Studio/Rider）添加对安装在服务器上的 `CounterStrikeSharp.Api.dll` 文件的引用。如果您使用的是 VSCode 或文本编辑器，可以直接编辑 .csproj 文件并添加以下内容：

```diff
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

+  <ItemGroup>
+    <Reference Include="CounterStrikeSharp.API">
+       <HintPath>[where you downloaded or installed]/addons/counterstrikesharp/api/CounterStrikeSharp.API.dll</HintPath>
+    </Reference>
+  </ItemGroup>
</Project>
```

> [!TIP]
> 不要手动添加对 CounterStrikeSharp.Api.dll 的引用，您可以使用以下命令安装 NuGet 包 CounterStrikeSharp.Api：
> ```shell
> dotnet add package CounterStrikeSharp.API
> ```

### 创建插件文件

将新项目中默认的类文件（默认应为 `Class1.cs`）重命名为更准确的名称，例如 `HelloWorldPlugin.cs`。在此文件中，我们将插入示例 Hello World 插件。请确保更改名称和命名空间，使其与您的项目名称匹配。

```csharp
using CounterStrikeSharp.API.Core;

namespace HelloWorldPlugin;
public class HelloWorldPlugin : BasePlugin
{
    public override string ModuleName => "Hello World Plugin";

    public override string ModuleVersion => "0.0.1";

    public override void Load(bool hotReload)
    {
        Console.WriteLine("Hello World!");
    }
}
```

现在，使用您的 IDE 或 `dotnet build` 命令构建项目。此时，您的项目应在 `bin/Debug/net8.0` 子目录中生成一个构建好的二进制文件。

### 安装你自己的插件

找到 CS2 专用服务器中的 `plugins` 文件夹（`/game/csgo/addons/counterstrikesharp/plugins`），并创建一个与您的输出 .dll 文件名称完全相同的新文件夹。在此示例中，文件名为 `HelloWorldPlugin.dll`，因此我们将创建一个名为 `HelloWorldPlugin` 的新文件夹。在该文件夹中，复制并粘贴以下文件：`HelloWorldPlugin.deps.json`、`HelloWorldPlugin.dll` 和 `HelloWorldPlugin.pdb`。完成后，文件夹结构应如下所示：

```shell
.
└── HelloWorldPlugin
    ├── HelloWorldPlugin.deps.json
    ├── HelloWorldPlugin.dll
    └── HelloWorldPlugin.pdb
```

> [!CAUTION]
> 如果您的插件安装了外部 NuGet 包，则可能需要包含它们各自的 `.dll` 文件。例如，如果您使用了 `Stateless` C# 库，请将 `stateless.dll` 包含在 `HelloWorld` 插件目录中。

> [!NOTE]
> 请注意，根据所使用的 CounterStrikeSharp 版本，一些依赖项可能会有所不同。

### 启动你的游戏服务器

现在启动您的 CS2 专用服务器。在显示 `CounterStrikeSharp.API Loaded Successfully.` 消息之前，您应该会看到从加载函数调用的 `Hello World!` 控制台输出，恭喜你，你已经完成了第一个插件的编写与使用！


> [!NOTE]
> 默认情况下，如果您替换插件文件夹中的 .dll 文件，CounterStrikeSharp 将自动热重载您的插件。此时，它会分别调用 `Unload` 和 `Load` 函数，并将 `hotReload` 标志设置为 true。
> 
> 值得注意的是，框架会自动为您取消注册任何事件处理程序或监听器，因此您可以在加载时安全地重新注册这些，而无需检查该标志。不过，如果您希望在热重载时执行一些特定操作，可以在 `Load()` 调用中通过检查该标志来实现！
