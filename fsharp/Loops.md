# F#: Loops


```fsharp
let http url=
    ""
let repeatFetch url n=
    for i=1 to n do
        let html = http url
        printfn "fetched <<< %s >>>" html
    printfn "Done!"

open System
let loopUntilSaturday() =
    while(DateTime.Now.DayOfWeek<>DayOfWeek.Saturday) do
        printfn "Still working"
    printfn "Saturday at last!"

for (b,pj) in [("Banana 1",false);("Banana 2",true)] do
    if pj then
        printfn "%s is n pyjamas today!" b;;

open System.Text.RegularExpressions

for m in Regex.Matches("All the Pretty Horses","[a-zA-Z]+") do
    printfn "res = %s" m.Value
```

