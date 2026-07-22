# .NET

## Links

* [Awesome .NET libraries](https://github.com/quozd/awesome-dotnet)
* [Awesome lists (sindresorhus)](https://github.com/sindresorhus/awesome)
* [Entity Framework Fluent API - Configuring Properties and Types](https://msdn.microsoft.com/en-us/data/jj591617.aspx#Model)
* [Fluent API in Code-First tutorial](http://www.entityframeworktutorial.net/code-first/fluent-api-in-code-first.aspx)
* [FluentMigrator - Easy database migrations with C#](https://iainhunter.wordpress.com/2012/08/02/easy-database-migrations-with-c-and-fluentmigrator/)
* [Managing Multiple Configuration Environments with Pre-Build Events](http://www.hanselman.com/blog/ManagingMultipleConfigurationFileEnvironmentsWithPreBuildEvents.aspx)
* [NuGet Package - ChameleonForms for ASP.NET MVC](http://www.hanselman.com/blog/NuGetPackageOfTheWeekADifferentTakeOnASPNETMVCFormsWithChameleonForms.aspx)
* [C# 6 Roslyn](https://github.com/dotnet/roslyn)
* [SpecFlow NUnit and Selenium example](https://github.com/mateusleonardi/SpecFlowNUnitSelenium)
* [.NET Workflow States (MSDN)](http://msdn.microsoft.com/pt-br/magazine/cc163281.aspx)
* [Fire and Forget with C# Async Task](http://blog.stephencleary.com/2014/06/fire-and-forge)
* [Revista Programar - C# Articles](https://www.revista-programar.info/categoria/artigos/colunas/csharp/)
* [.NET Documentation](https://learn.microsoft.com/en-us/dotnet/)
* [C# Language Reference](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/)
* [NuGet Documentation](https://learn.microsoft.com/en-us/nuget/)
* [Entity Framework Core Documentation](https://learn.microsoft.com/en-us/ef/core/)
* [ASP.NET Core Documentation](https://learn.microsoft.com/en-us/aspnet/core/)
* [LINQ (Language-Integrated Query)](https://learn.microsoft.com/en-us/dotnet/csharp/linq/)

## Discover Public Key Token of a DLL

```powershell
([system.reflection.assembly]::loadfile("c:\MyDLL.dll")).FullName
```

## Register Windows Assembly (GAC) on Windows Server 2008

1. Open `cmd` as Administrator
2. Navigate to `C:\Program Files (x86)\Microsoft Visual Studio 8\SDK\v2.0\Bin`
3. Run:

```sh
gacutil /i <path\name_of_dll>
```

## Excel File Locked by IIS via Interop

**Error:**
```
System.Runtime.InteropServices.COMException (0x800A03EC): Microsoft Office Excel cannot access the file
```

**Fix:** Create the Desktop directory for the IIS user:

- 64-bit Windows: `C:\Windows\SysWOW64\config\systemprofile\Desktop`
- 32-bit Windows: `C:\Windows\System32\config\systemprofile\Desktop`

Set Full Control permissions on the `Desktop` folder for `IIS AppPool\DefaultAppPool`.

## Sharepoint Safe Control Tag Example

```xml
<SafeControl Assembly="MyCompany.MyAssembly, Version=1.0.0.0, Culture=neutral, PublicKeyToken=c738ecee6c22773b" Namespace="MyCompany.MyAssembly" TypeName="*" Safe="True" />
```

## Generate C# Classes from JSON

Online tool: [json2csharp.com](http://json2csharp.com/)

Or use Visual Studio 2012+:

1. Copy the JSON (`Ctrl+C`)
2. In the `.cs` file, go to Edit > Paste Special > Paste JSON as Classes

## Visual Studio Shortcuts


## Visual Studio 2008 HTML Highlight Fix

Open the Visual Studio 2008 Developer Command Prompt and run:

```sh
devenv /ResetSkipPkgs
```
