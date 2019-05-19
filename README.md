# MSBuild.Sdk.Cpp

This repository contains a prototype of building a C++ UWP app with the [C# project system](https://github.com/dotnet/project-system) using an MSBuild SDK that calls into the C++ MSBuild targets that ship with Visual Studio. It is based on [MSBuild.Sdk.Extras](https://github.com/onovotny/MSBuildSdkExtras). It enables C++ UWP projects to make use of MSBuild globbing and NuGet PackageReference.

Currently the project file looks like:

```msbuild
<Project ToolsVersion="Current">

    <PropertyGroup>
        <CppWinRTOptimized>true</CppWinRTOptimized>
        <MinimalCoreWin>true</MinimalCoreWin>
        <ConfigurationType>Application</ConfigurationType>
        <AppContainerApplication>true</AppContainerApplication>
        <ApplicationType>Windows Store</ApplicationType>
        <ApplicationTypeRevision>10.0</ApplicationTypeRevision>
        <WindowsTargetPlatformVersion Condition=" '$(WindowsTargetPlatformVersion)' == '' ">10.0.17763.0</WindowsTargetPlatformVersion>
        <WindowsTargetPlatformMinVersion>10.0.17763.0</WindowsTargetPlatformMinVersion>
    </PropertyGroup>

    <Import Project="$(MSBuildThisFileDirectory)..\..\src\MSBuild.Sdk.Cpp\Sdk\Sdk.props" />

    <ItemGroup>
        <PackageReference Include="Microsoft.Windows.CppWinRT" Version="2.0.190506.1" />
    </ItemGroup>

    <Import Project="$(MSBuildThisFileDirectory)..\..\src\MSBuild.Sdk.Cpp\Sdk\Sdk.targets" />

</Project>
```

# Should I use this?

Probably not. Even though NuGet restore and building works, various other things do NOT work:
- Running/deploying the app from VS.
- Property sheets.
- Intellisense.
- Xaml Designer.
- Your project files need to be .csproj.

This means you manually have to edit the project files if you want to change properties, you won't have auto-complete, and debugging is complicated. Furthermore, none of this is supported in anyway, so there is no guarantee this will work and bugs might not be fixed.

# Is there a NuGet package?

There is not one today.