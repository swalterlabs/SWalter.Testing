<!-- markdownlint-disable MD033 -->
# SWalter.Testing

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)
[![Target: .NET 10.0](https://img.shields.io/badge/.NET-10.0-512bd4.svg?style=for-the-badge&logo=dotnet)](Directory.Build.props)

Testing utilities, AutoFixture extensions, custom assertions, and AwesomeAssertions extensions for .NET and Microsoft.Extensions.DependencyInjection.

---

## 📈 Quality & Activity

| Branch                                                                                                                | Status & Activity                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| :-------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ![Main](https://img.shields.io/badge/branch-main-brightgreen?style=flat-square&logo=git&logoColor=white&color=c9ff00) | [![CodeFactor](https://www.codefactor.io/repository/github/swalterlabs/SWalter.Testing/badge/main?style=flat-square)](https://www.codefactor.io/repository/github/swalterlabs/SWalter.Testing/overview/main) ![Commit Activity Main](https://img.shields.io/github/commit-activity/m/swalterlabs/SWalter.Testing/main?style=flat-square) ![Last Commit Main](https://img.shields.io/github/last-commit/swalterlabs/SWalter.Testing/main?style=flat-square)                   |
| ![Develop](https://img.shields.io/badge/branch-develop-blue?style=flat-square&logo=git&logoColor=white&color=0080ff)  | [![CodeFactor](https://www.codefactor.io/repository/github/swalterlabs/SWalter.Testing/badge/develop?style=flat-square)](https://www.codefactor.io/repository/github/swalterlabs/SWalter.Testing/overview/develop) ![Commit Activity Develop](https://img.shields.io/github/commit-activity/m/swalterlabs/SWalter.Testing/develop?style=flat-square) ![Last Commit Develop](https://img.shields.io/github/last-commit/swalterlabs/SWalter.Testing/develop?style=flat-square) |

---

## 📦 Packages in this Repository

| Package                                  | Description                                                                              | Sources                                                                                                                                                                                                                                                                                                         |
| :--------------------------------------- | :--------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`SWalter.Testing`](src/SWalter.Testing) | AutoFixture custom data attributes, GuardClause assertions, and DI assertion extensions. | [![MyGet](https://img.shields.io/badge/MyGet-gray?style=flat-square&logo=myget)](https://myget.org/feed/swalterlabs/package/nuget/SWalter.Testing) [![Codeberg](https://img.shields.io/badge/Codeberg-gray?style=flat-square&logo=codeberg)](https://codeberg.org/evilbaschdi/-/packages/nuget/SWalter.Testing) |

---

## 🚀 Package Feeds

All packages (Release and Preview builds) are published to **MyGet** and **Codeberg**. You only need to configure **one** of these feeds.

| Registry     | Feed URL                                                         |
| :----------- | :--------------------------------------------------------------- |
| **MyGet**    | `https://www.myget.org/F/swalterlabs/api/v3/index.json`          |
| **Codeberg** | `https://codeberg.org/api/packages/evilbaschdi/nuget/index.json` |

### Add Feed via .NET CLI

Choose either MyGet or Codeberg:

```bash
# Option A: MyGet (recommended)
dotnet nuget add source https://www.myget.org/F/swalterlabs/api/v3/index.json -n "SWalter Labs"

# Option B: Codeberg
dotnet nuget add source https://codeberg.org/api/packages/evilbaschdi/nuget/index.json -n "EvilBaschdi Codeberg"
```

<details>
<summary><b>Sample <code>NuGet.Config</code> with Package Source Mapping</b></summary>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <clear />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
    <add key="SWalter Labs" value="https://www.myget.org/F/swalterlabs/api/v3/index.json" />
  </packageSources>

  <packageSourceMapping>
    <packageSource key="nuget.org">
      <package pattern="*" />
    </packageSource>
    <packageSource key="SWalter Labs">
      <package pattern="SWalter.*" />
    </packageSource>
  </packageSourceMapping>
</configuration>
```

</details>

---

## 📥 Installation

Install any package via `dotnet add package`:

### Standard Release

```bash
dotnet add package SWalter.Testing
```

### Preview Builds

```bash
dotnet add package SWalter.Testing --prerelease
```

---

## 💡 Features & Usage

### AutoFixture Attributes

#### `NSubstituteOmitAutoPropertiesTrueAutoDataAttribute`

A custom xUnit `[Theory]` attribute combining AutoFixture's `AutoDataAttribute` with NSubstitute automatic mocking and `OmitAutoProperties = true`.

```csharp
using SWalter.Testing;
using Xunit;

[Theory, NSubstituteOmitAutoPropertiesTrueAutoData]
public void MyTest(IMyService service, MyDependency dependency)
{
    // service and dependency are automatically created and injected
    // properties are not auto-populated - configure them as needed
}
```

#### `NSubstituteOmitAutoPropertiesTrueInlineAutoDataAttribute`

Combines `InlineAutoDataAttribute` with NSubstitute automatic mocking and `OmitAutoProperties = true`.

```csharp
using SWalter.Testing;
using Xunit;

[Theory]
[NSubstituteOmitAutoPropertiesTrueInlineAutoData("value1", 42)]
public void MyTest(string inlineValue, int inlineNumber, IMyService service)
{
    // inlineValue and inlineNumber come from the attribute parameters
    // service is automatically created by the fixture
}
```

### Guard Clause Assertions

Verifies that public asynchronous methods on a type have proper null guards for reference parameters:

```csharp
using SWalter.Testing.Extensions;
using AutoFixture.Xunit3;

[Theory, NSubstituteOmitAutoPropertiesTrueAutoData]
public void VerifyAllAsyncMethodsHaveNullGuards(GuardClauseAssertion assertion)
{
    assertion.VerifyTask<MyAsyncService>(
        typeof(MyAsyncService).GetMethods(System.Reflection.BindingFlags.Public | System.Reflection.BindingFlags.Instance));
}
```

### Assertions for `Microsoft.Extensions.DependencyInjection`

Assert service registrations with lifetime and implementation validations:

```csharp
using SWalter.Testing;
using AwesomeAssertions;
using Microsoft.Extensions.DependencyInjection;
using SWalter.Testing.Assertions.Microsoft.Extensions.DependencyInjection;

var services = new ServiceCollection();
services.AddSingleton<ISomeService, SomeService>();
services.AddTransient<ITransient, Transient>();
services.AddScoped<IScoped, Scoped>();

// Assert registration, implementation, and lifetime
services.Should()
    .HaveService<ISomeService>()
    .WithImplementation<SomeService>()
    .AsSingleton();
```
