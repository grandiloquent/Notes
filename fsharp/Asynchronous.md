# F#: Asynchronous

```
open System.IO
open System.Net

let downloadPage (url : string) =
    async {
        let req = HttpWebRequest.Create(url)
        use! resp = req.AsyncGetResponse()
        use respStream = resp.GetResponseStream()
        use sr = new StreamReader(respStream)
        return sr.ReadToEnd()
    }

type Downloader() =
    let beginMethod, endMethod, cancelMethod =
        Async.AsBeginEnd downloadPage
    member this.BeginDownload(url, callback, state : obj) =
        beginMethod (url, callback, state)
    member this.EndDownload(ar) =
        endMethod ar
    member this.CancelDownload(ar) =
        cancelMethod (ar)

downloadPage ("https://www.bing.com")
 |> Async.RunSynchronously



open System
type MyEvent(v : string) =
        inherit EventArgs()
        member this.Value = v;

    let testAwaitEvent (evt : IEvent<MyEvent>) = async {
        printfn "Before waiting"
        let! r = Async.AwaitEvent evt
        printfn "After waiting: %O" r.Value
        do! Async.Sleep(1000)
        return ()
        }

     let runAwaitEventTest () =
        let evt = new Event<Handler<MyEvent>, _>()
        Async.Start <| testAwaitEvent evt.Publish
        System.Threading.Thread.Sleep(3000)
        printfn "Before raising"
        evt.Trigger(null, new MyEvent("value"))
        printfn "After raising"

        runAwaitEventTest()

```fsharp

