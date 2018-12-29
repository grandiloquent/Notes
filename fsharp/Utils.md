# F#: Utils


```fsharp

namespace Utils
// substring after
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
```


