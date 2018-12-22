# F# Delete specific file 

```fsharp
open System.Reflection
open System.IO

let WorkingDirectory = Path.GetDirectoryName(Assembly.GetEntryAssembly().Location)
let GetFiles path = Directory.GetFiles(path, "*", SearchOption.AllDirectories)

let listFile =
   let files = GetFiles("C:\\Users\\psycho\\Desktop\\Safari\\666")
   let ariaFiles = files |> Array.filter (fun x -> Path.GetExtension(x) = ".aria2");
   let filterNames = ariaFiles |> Array.map (fun x -> Path.GetFileNameWithoutExtension(x))
   files |> Array.filter (fun x -> filterNames |> Array.contains (Path.GetFileNameWithoutExtension(x)))
         |> Array.iter (File.Delete)
None

[<EntryPoint>]
let main argv =
    listFile;
    0 // return an integer exit code

```
