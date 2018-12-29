# F#: 下载 Safari 书籍


```fsharp
// nuget install FSharp.Data -Version 3.0.0

#r"FSharp.Data.3.0.0\\lib\\net45\\FSharp.Data.dll"

open System
open System.Text.RegularExpressions
open System.IO
open System.Text
open FSharp.Data
open FSharp.Data.JsonExtensions
open System.Diagnostics
open System.Threading
#load "Utils.fsx"

open Utils.Operators
open Utils




type Book=
    struct
        val Title:string
        val Url:string

        val DateAdd:DateTime
        new (title:string,url:string,dataAdd)={Title=title;Url=url;DateAdd=dataAdd}
        override b.ToString()=sprintf "%s" b.Title
    end
let fetchSearchJson=
    async{

        return! Http.AsyncRequestString(
                   "https://www.safaribooksonline.com/api/v2/search/",
                   httpMethod="POST",
                   headers= [  "Content-Type", HttpContentTypes.Json ],
                   body=TextRequest """ {"query":"*","extended_publisher_data":"true","highlight":"true","is_academic_institution_account":"false","source":"user","include_assessments":"false","include_case_studies":"true","include_courses":"true","include_orioles":"true","include_playlists":"true","formats":["book"],"topics":[],"publishers":[],"languages":[],"sort":"date_added","field":"title"} """)
    }|>Async.Catch






let createDirectoryIfNotExists(dir:string)=
    if not(Directory.Exists(dir)) then
        Directory.CreateDirectory(dir)|>ignore
let dir=
    let d= Path.Combine(Environment.GetFolderPath(
                            Environment.SpecialFolder.Desktop),"Books")
    createDirectoryIfNotExists(d)
    Log.green ("Books will be saved to: "+d)
    d
let createDirectory(node:HtmlDocument)=
    let title=node.CssSelect("title")
    if title.IsEmpty then
        Log.red "node does not include the title tag"
        null
    else
        let t=title.Head.InnerText()
        let n=Path.Combine(dir,(t--|'['>|<" ").TrimEnd())
        Log.green ("The book's title is: " + n)
        createDirectoryIfNotExists(n)
        n
let createTableContentFile(dir:String,node:HtmlNode)=
    let t=Regex.Replace( node.ToString(), @"\s*class=""[^""]*?""\s*(?=>)","")
    File.WriteAllText(Path.Combine(dir,"目录.html"),t,new UTF8Encoding(false));
let createLinksFile(dir:String,node:HtmlNode)=
    let links=node.CssSelect("a")|>List.map(fun x-> match x.TryGetAttribute("href") with
                                                    |Some x -> "https://www.oreilly.com/"+(x.Value()|-"com/")
                                                    |None->"")

    File.WriteAllLines(Path.Combine(dir,"links.txt"),links,new UTF8Encoding(false))
    Path.Combine(dir,"links.txt")
let invokeAria(fileName:string)=
    let startInfo=new ProcessStartInfo()
    startInfo.FileName<-"aria2c"
    startInfo.WorkingDirectory<-Path.GetDirectoryName(fileName)
    startInfo.Arguments<-"--input-file=\""+fileName+"\" --load-cookies=\""+Path.Combine(dir,"cookie.txt")+"\""
    Log.green startInfo.Arguments
    let p=Process.Start startInfo
    p.WaitForExit()
    ()

let fetchBook url=
    async{
        Log.green "Starting download book"
        let! html=Http.AsyncRequestString(url)
        let hd= HtmlDocument.Parse(html)
        let d=createDirectory(hd)
        let toc = hd.CssSelect(".detail-toc").Head
        createTableContentFile(d,toc)
        let linkFile=createLinksFile(d,toc)
        invokeAria linkFile

        //printfn "%s" ()
        ()
    }
fetchBook "https://www.oreilly.com/library/view/arduino-applied-comprehensive/9781484239605/"|>Async.RunSynchronously
// let result=queryJson|>Async.RunSynchronously
// let books = match result with
//             | Choice1Of2 text->let j=JsonValue.Parse(text)
//                                Some (j?results.AsArray()
//                                 |>Array.filter(fun x->x?content_type.AsString()="book")
//                                 |>Array.map(fun x-> new Book(x?title.AsString(),x?web_url.AsString(),x?date_added.AsDateTime()))
//                                 |>Array.sortByDescending(fun x->x.DateAdd))

//             | Choice2Of2 e->None

// match books with
//     |Some x-> x|>Array.iter(printfn "%A")
//     |None -> ()

```


