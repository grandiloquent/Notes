# C#

### Async main


```
static async Task<int> Main()
{
    // This could also be replaced with the body
    // DoAsyncWork, including its await expressions:
    return await DoAsyncWork();
}
```

## Default literal expressions


## NuGet

	nuget install Npgsql -Version 1

## .NET Core

- https://dotnet.microsoft.com/download

### dotnet

https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet

- dotnet add package HtmlAgilityPack --version 1.8.11
- dotnet new console -lang C# -n CreateFiles
- dotnet publish -c Release -r win7-x64 --self-contained=false
- dotnet clean --configuration Release

### CoreRT

1.  `dotnet new nuget` 

	Open the file and in the  `<packageSources>`  element under  `<clear/>`  add the following:

		<add key="dotnet-core" value="https://dotnet.myget.org/F/dotnet-core/api/v3/index.json" />
		<add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />	
2.  `dotnet add package Microsoft.DotNet.ILCompiler -v 1.0.0-alpha-*` 

		https://dotnetmyget.blob.core.windows.net/artifacts/dotnet-core/nuget/v3/flatcontainer/microsoft.dotnet.ilcompiler/index.json

3.  `dotnet publish -r win-x64 -c release` 

- https://github.com/dotnet/corert/tree/master/samples/WebApi
- https://github.com/dotnet/corert

- https://www.nuget.org/
- https://www.jetbrains.com/rider/download/#section=windows

