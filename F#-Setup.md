# F# Setup

F# > version 4.1 is required for running DAXIF. Follow the instructions on the following page for installing: [Use F# on Windows](https://fsharp.org/use/windows/).

> Visual Studio 2019 comes with F# support in all its editions

If you are still using Visual Studio 2017, you can enable this in the Visual Studio installer:
> In the “Individual components” tab: under the “Development activities” category, check “F# language support”.

If you are using Visual Studio 2022, you need to disable `Use .Net Core Scripting`
> This is done by opening VS2022 and go to the “F# Interactive” tab: under the `Tools`, `Options`, `F# Tool`, `F# Interactive` and changing `Use .Net Core Scripting` to `False`.
> Reset VS2022 or right-click `F# Interactive` window and select `Reset Interactive Session`.

If you are using Daxif v5.1.1 or later (maybe as low as v4.7.2) the vm image `windows-2019` wont work
> Use the vm image `windows-2022` or `windows-latest` instead