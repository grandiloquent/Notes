# F#: Utils


```fsharp
namespace Utils

open System
open System.Threading
open System.Text.RegularExpressions

module Log =


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

module Operators =
    // substring before last
    let inline (--|) (s:string)(c:char)=
        let i=s.LastIndexOf(c)
        if (-1=i) then
            s
        else
            s.Substring(0,i)
    let inline (|-) (s:string)(c:string)=
        let i=s.IndexOf(c)
        if (-1=i) then
            s
        else
            s.Substring(i+c.Length)

    let (>|<) (s:string)(c:string)=
        Regex.Replace(s,@"[:*?/\""<>|\0\u0001\u0002\u0003\u0004\u0005\u0006\u0007\u0008\u0009\u000a\u000b\u000c\u000d\u000e\u000f\u0010\u0011\u0012\u0013\u0014\u0015\u0016\u0017\u0018\u0019\u001a\u001b\u001c\u001d\u001e\u001f]",c)

```


