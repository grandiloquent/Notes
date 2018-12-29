# F#: Records

```
let http url=
    ""
type DiscreteEventCounter=
    { mutable Total:int;
      mutable Positive:int;
      Name:string
    }

let recordEvent(s:DiscreteEventCounter) isPositive=
    s.Total<-s.Total+1
    if isPositive then s.Positive<-s.Positive+1
let reportStatus(s:DiscreteEventCounter)=
    printfn "We have %d %s out of %d" s.Positive s.Name s.Total
let newCounter nm=
    { Total=0;
      Positive=0;
      Name=nm
    }
let longPageCounter=newCounter "long page(s)"
let fetch url=
    let page=http url
    recordEvent longPageCounter (page.Length>10000)
    page

fetch "http://www.bing.com/"|>ignore

reportStatus longPageCounter
```fsharp

