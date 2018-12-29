# F#: Change filename recursively

```fsharp
open System.IO

let rec allFilesUnder baseFolder=
    seq{
        yield! Directory.GetFiles baseFolder
        for subDir in Directory.GetDirectories baseFolder do
            yield! allFilesUnder subDir
    }
let (|EndsWith|_|) extension (file:string)=
    if file.EndsWith(extension)
    then Some()
    else None

let changeFileName (fileName:string)=
    let newFileName=sprintf "%sl" fileName
    File.Move(fileName,newFileName)
    printfn "%s" newFileName
let dir="""C:\Users\psycho\Desktop\Books\其他"""

allFilesUnder dir
|>Seq.filter(function
            |EndsWith ".htm" _
               -> true
            |_ -> false)
|>Seq.iter(changeFileName)
```

