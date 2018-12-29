# F#: Compress Directory

```
#r"DotNetZip.1.13.0/lib/net40/DotNetZip.dll"
open System.IO
open Ionic.Zip
open System.Text

// nuget Install DotNetZip -Version 1.13.0
// dotnet fsi fs.fsx
let FolderUnder baseFolder=
    seq{
        yield! Directory.GetDirectories baseFolder
    }

let CompressDirectory (dir:string)=
    printfn "%s" dir
    use zip=new ZipFile(Encoding.GetEncoding("gbk"))
    zip.AddDirectory(dir,"")|>ignore
    zip.Save(dir+".epub")
    ()
let dir="""C:\Users\psycho\Desktop\Books\其他"""

FolderUnder dir
|>Seq.iter(CompressDirectory)
```fsharp

