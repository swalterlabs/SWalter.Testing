# SWalter.Testing

This package offers custom NSubstitute AutoData attributes and the required NuGet references for unit tests.

## Usage

(Example uses Global Package Management)

```xml
<!-- Optional: disable auto global usings imported by SWalter.Testing.props -->
<PropertyGroup>
  <SWalterTestingApplyTestDefaults>false</SWalterTestingApplyTestDefaults>
  <SWalterTestingEnableGlobalUsings>false</SWalterTestingEnableGlobalUsings>
</PropertyGroup>

<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>

<ItemGroup>
    <Using Include="AutoFixture" />
    <Using Include="AutoFixture.AutoNSubstitute" />
    <Using Include="AutoFixture.Idioms" />
    <Using Include="AutoFixture.Xunit3" />
    <Using Include="SWalter.Testing" />
    <Using Include="SWalter.Testing.Assertions.Microsoft.Extensions.DependencyInjection" />
    <Using Include="AwesomeAssertions" />
    <Using Include="NSubstitute" />
    <Using Include="NSubstitute.Arg" Alias="Arg" />
    <Using Include="NSubstitute.ReturnsExtensions" />
    <Using Include="Xunit" />
</ItemGroup>

<ItemGroup>
    <PackageReference Include="SWalter.Testing" />
    <PackageReference Include="xunit.v3.mtp-v2" />
    <PackageReference Include="Microsoft.NET.Test.Sdk" />
    <PackageReference Include="coverlet.collector">
        <PrivateAssets>all</PrivateAssets>
        <IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
    </PackageReference>
    <PackageReference Include="AwesomeAssertions.Analyzers">
        <PrivateAssets>all</PrivateAssets>
        <IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
    </PackageReference>
    <PackageReference Include="NSubstitute.Analyzers.CSharp">
        <PrivateAssets>all</PrivateAssets>
        <IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
    </PackageReference>
    <PackageReference Include="xunit.analyzers">
        <PrivateAssets>all</PrivateAssets>
        <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
    </PackageReference>
    <PackageReference Include="xunit.runner.visualstudio">
        <IncludeAssets>runtime; build; native; contentfiles; analyzers</IncludeAssets>
        <PrivateAssets>all</PrivateAssets>
    </PackageReference>
</ItemGroup>
```
