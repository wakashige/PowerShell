# Build PowerShell on Windows for .NET Core

This guide will walk you through building PowerShell on Windows, targeting .NET Core.
We'll start by showing how to set up your environment from scratch.

## Environment

These instructions are tested on Windows 10 and Windows Server 2012
R2, though they should work anywhere the dependencies work.

### Git Setup

Using Git requires it to be setup correctly; refer to the
[README](../../README.md) and
[Contributing Guidelines](../../.github/CONTRIBUTING.md).

This guide assumes that you have recursively cloned the PowerShell repository and `cd`ed into it.

### Visual Studio

You will need to install an edition of Visual Studio 2015 (Community, Enterprise, or Professional) with the optional feature 'Common Tools for Visual C++' installed.
The free Community edition of Visual Studio 2015 can be downloaded [here](https://www.visualstudio.com/visual-studio-community-vs/).

### Visual Studio Code

Building PowerShell using [Visual Studio Code](https://code.visualstudio.com/) depends on the PowerShell executable to be called `pwsh` which means
that you must have PowerShell Core 6 Beta.9 (or newer) installed to successfully build this project (typically for the purpose of debugging).

### .NET CLI

We use the [.NET Command Line Interface][dotnet-cli] (`dotnet`) to build PowerShell.
The version we are currently using is `2.0.0`.
The `Start-PSBootstrap` function will automatically install it and add it to your path:

```powershell
Import-Module ./build.psm1
Start-PSBootstrap
```

Or you can call `Install-Dotnet` directly:

```powershell
Install-Dotnet
```

It removes the previously installed version of .NET CLI and install the version that PowerShell Core depends on.
If you have any problems installing `dotnet`, please see their [documentation][cli-docs].

[dotnet-cli]: https://github.com/dotnet/cli
[cli-docs]: https://www.microsoft.com/net/core#windowscmd

## Build using our module

We maintain a [PowerShell module](../../build.psm1) with the function `Start-PSBuild` to build PowerShell.

```powershell
Import-Module ./build.psm1
Start-PSBuild
```

Congratulations! If everything went right, PowerShell is now built and executable as `./src/powershell-win-core/bin/Debug/netcoreapp2.1/win7-x64/publish/pwsh.exe`.

This location is of the form `./[project]/bin/[configuration]/[framework]/[rid]/publish/[binary name]`,
and our project is `powershell`, configuration is `Debug` by default,
framework is `netcoreapp2.1`, runtime identifier is `win7-x64` by default,
and binary name is `pwsh`.
The function `Get-PSOutput` will return the path to the executable;
thus you can execute the development copy via `& (Get-PSOutput)`.

The `powershell` project is the .NET Core PowerShell host.
It is the top level project, so `dotnet build` transitively builds all its dependencies,
and emits a `pwsh` executable.
The cross-platform host has built-in documentation via `--help`.

You can run our cross-platform Pester tests with `Start-PSPester`.

## Building in Visual Studio

We currently have the issue [#3400](https://github.com/PowerShell/PowerShell/issues/3400) tracking this task.
