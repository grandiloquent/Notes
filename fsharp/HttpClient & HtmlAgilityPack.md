# F#: HttpClient & HtmlAgilityPack

```
open System.Windows.Forms
open System
#r "System.Net.Http.4.3.4/lib/net46/System.Net.Http.dll"
#r "HtmlAgilityPack.1.8.11/lib/Net45/HtmlAgilityPack.dll"
open System.IO
open System.Net.Http;
open System.Net
open HtmlAgilityPack


// nuget Install System.Net.Http -Version 4.3.4
// nuget Install HtmlAgilityPack -Version 1.8.11


let strip chars = String.map (fun c -> if Array.exists((=)c) chars then ' ' else c)



let fetch (url:string)=
    printfn "%s" url
    ServicePointManager.SecurityProtocol <- SecurityProtocolType.Tls12 ||| SecurityProtocolType.Tls11 ||| SecurityProtocolType.Tls
    async{
        try
            let handler=new HttpClientHandler()
            handler.UseProxy<-false
            handler.UseCookies<-false
            let httpClient=new HttpClient(handler)
            let! response=httpClient.GetAsync(url)|>Async.AwaitTask
            response.EnsureSuccessStatusCode()|>ignore
            let! content=response.Content.ReadAsStringAsync()|>Async.AwaitTask
            return content
        with
            |ex ->
                printfn "%s" (ex.Message)
                return null

    }

let chars=Path.GetInvalidFileNameChars()
let r=Clipboard.GetText()|>fetch|>Async.RunSynchronously
let hd=new HtmlDocument()
hd.LoadHtml(r)
let title=hd.DocumentNode.SelectSingleNode("//title").InnerText.Split('[').[0].Trim()
printfn "%s"(strip chars title)
Console.ReadKey()


```fsharp

