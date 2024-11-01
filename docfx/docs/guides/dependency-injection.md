---
title: 依赖注入
description: 如何在CounterStrikeSharp中使用依赖注入
---

# 依赖注入

如何在CounterStrikeSharp中使用依赖注入

`CounterStrikeSharp` 使用标准的 <a href="https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-8.0" target="_blank">`IServiceCollection`</a> 来允许在插件中进行依赖注入。

有一些基本服务已经预置了（例如用于日志记录的 `ILogger`），并将在未来增加更多预置服务。要向容器中添加自定义的` scoped` 和 `singleton` 服务，可以为您的插件创建一个实现`IPluginServiceCollection<T>`接口的新类。

```csharp
public class TestPlugin : BasePlugin
{
  // Plugin code...
}

public class TestPluginServiceCollection : IPluginServiceCollection<TestPlugin>
{
    public void ConfigureServices(IServiceCollection serviceCollection)
    {
        serviceCollection.AddScoped<ExampleInjectedClass>();
        serviceCollection.AddLogging(builder => ...);
    }
}
```
CounterStrikeSharp 将检索当前程序中所有的 `IPlugin` 和 `IPluginServiceCollection<T>` ，其中 `T` 是您的插件。接着，它将配置服务程序，并在进行加载步骤之前请求您的插件的单例实例。

通过这种方式，您插件类构造函数中列出的任何依赖项将在实例化时（加载之前）自动注入。

### Example

```csharp
public class TestInjectedClass
{
    private readonly ILogger<TestInjectedClass> _logger;

    public TestInjectedClass(ILogger<TestInjectedClass> logger)
    {
        _logger = logger;
    }

    public void Hello()
    {
        _logger.LogInformation("Hello World from Test Injected Class");
    }
}

public class TestPluginServiceCollection : IPluginServiceCollection<SamplePlugin>
{
    public void ConfigureServices(IServiceCollection serviceCollection)
    {
        serviceCollection.AddScoped<TestInjectedClass>();
    }
}

public class SamplePlugin : BasePlugin
{
    private readonly TestInjectedClass _testInjectedClass;
    public SamplePlugin(TestInjectedClass testInjectedClass)
    {
        _testInjectedClass = testInjectedClass;
    }

    public override void Load(bool hotReload)
    {
        _testInjectedClass.Hello();
    }
}
```
