# F#: Use paket.exe

https://stackoverflow.com/questions/31148101/how-to-use-nuget-install-package-for-f-script-without-a-solution

```
// Step 0. Boilerplate to get the paket.exe tool
open System
open System.IO
Environment.CurrentDirectory <- __SOURCE_DIRECTORY__

if not (File.Exists "paket.exe") then
    let url = "https://github.com/fsprojects/Paket/releases/download/0.31.5/paket.exe"
    use wc = new Net.WebClient()
    let tmp = Path.GetTempFileName()
    wc.DownloadFile(url, tmp)
    File.Move(tmp,Path.GetFileName url)

// Step 1. Resolve and install the packages
#r "paket.exe"
Paket.Dependencies.Install """
source https://nuget.org/api/v2

nuget Suave
nuget FSharp.Data
nuget FSharp.Charting
""";;
```fsharp

