# F#: 下载 Safari 书籍


```fsharp

// nuget install FSharp.Data -Version 3.0.0

#r"FSharp.Data.3.0.0\\lib\\net45\\FSharp.Data.dll"
open FSharp.Data
open FSharp.Data.JsonExtensions
module Log =
     open System
     open System.Threading
     let report =
         let lockObj = obj()
         fun (color : ConsoleColor) (message : string) ->
             lock lockObj (fun _ ->
                 Console.ForegroundColor <- color
                 printfn "%s (thread ID: %i)"
                     message Thread.CurrentThread.ManagedThreadId
                 Console.ResetColor())
     let red = report ConsoleColor.Red
     let green = report ConsoleColor.Green
     let yellow = report ConsoleColor.Yellow
     let cyan = report ConsoleColor.Cyan
module Outcome =
    type Outcome =
        | OK of filename : string
        | Failed of filename : string
    let isOk = function
        | OK _ -> true
        | Failed _ -> false
    let fileName = function
        | OK name
        | Failed name -> name
module Download =
    open System
    open System
    open System.IO
    open System.Net
    open System.Text.RegularExpressions
    open FSharp.Data
    let private absoluteUri (pageUri : Uri) (filePath : string) =
        if filePath.StartsWith("http:")
           || filePath.StartsWith("https:") then
            Uri(filePath)
        else
            let sep = '/'
            filePath.TrimStart(sep)
            |> (sprintf "%O%c%s" pageUri sep)
            |> Uri
    let private getLinks (pageUri : Uri) (filePattern : string) =
        async {
            Log.cyan "Getting names..."
            let re = Regex(filePattern)
            let! html = HtmlDocument.AsyncLoad(pageUri.AbsoluteUri)
            let links =
                html.Descendants [ "a" ]
                |> Seq.choose (fun node ->
                    node.TryGetAttribute("href")
                    |> Option.map (fun att -> att.Value()))
                |> Seq.filter (re.IsMatch)
                |> Seq.map (absoluteUri pageUri)
                |> Seq.distinct
                |> Array.ofSeq
            return links
        }
    let AsyncGetFiles (pageUri : Uri) (filePattern : string) (localPath : string) =
        async {
            let! links = getLinks pageUri filePattern
            links |> Array.iter (fun x -> printfn "%s" (x.ToString()))
        }
// open System
// open System.Diagnostics
// open System.Net
// [<EntryPoint>]
// let main _ =
//     ServicePointManager.SecurityProtocol <- SecurityProtocolType.Tls12 ||| SecurityProtocolType.Tls11 ||| SecurityProtocolType.Tls

//     let uri = Uri @"https://www.safaribooksonline.com/search/?query=*&extended_publisher_data=true&highlight=true&is_academic_institution_account=false&source=user&include_assessments=false&include_case_studies=true&include_courses=true&include_orioles=true&include_playlists=true&formats=book&sort=date_added&field=title"
//     let pattern = @".*"
//     let localPath = @"c:\temp\downloads"
//     Download.AsyncGetFiles uri pattern localPath
//     |> Async.RunSynchronously
//     Console.ReadKey() |> ignore
//     0
open System
open System.Text.RegularExpressions
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
// substring before last
let inline (-|) (s:string)(c:char)=
    let i=s.LastIndexOf(c)
    printfn "%d" i
    if (-1=i) then
        s
    else
        s.Substring(0,i)
let stripInvalidFileNameChars(fileName:string)=
    Regex.Replace(fileName,@"[:*?/\""<>|\0\u0001\u0002\u0003\u0004\u0005\u0006\u0007\u0008\u0009\u000a\u000b\u000c\u000d\u000e\u000f\u0010\u0011\u0012\u0013\u0014\u0015\u0016\u0017\u0018\u0019\u001a\u001b\u001c\u001d\u001e\u001f]"," ")

let createDirectory(node:HtmlDocument)=
    let title=node.CssSelect("title")
    if title.IsEmpty then
        null
    else
        let t=title.Head.InnerText()
        let n=t-|'['|>stripInvalidFileNameChars
        printfn "%s" n
        n
let fetchBook url=
    async{
        Log.green "Starting download book"
        let! html=Http.AsyncRequestString(url)
        let hd= HtmlDocument.Parse(html)

        printfn "%s" (createDirectory(hd))
        //let toc = hd.CssSelect(".detail-toc").Head
        //printfn "%s" (Regex.Replace( toc.ToString(), @"\s*class=""[^""]*?""\s*(?=>)",""))
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


