# F#: Suave Web Server


```
open System
open System.Net
open Suave
open Suave.Sockets.Control
open Suave.Logging
open Suave.Operators
open Suave.EventSource
open Suave.Filters
open Suave.Writers
open Suave.Files
open Suave.Successful
open Suave.State.CookieStateStore

let basicAuth =
    Authentication.authenticateBasic ((=) ("foo", "bar"))
let loggingOptions =
  { Literate.LiterateOptions.create() with
         getLogLevelText = function | Verbose -> "V" | Debug -> "D" | Info -> "I" | Warn -> "W" | Error -> "E" | Fatal -> "F" }
let logger = LiterateConsoleTarget (
                                  name = [| "Suave"; "Examples"; "Example" |],
                                  minLevel = Warn,
                                  options = loggingOptions,
                                  outputTemplate = "[{level}] {timestampUtc:o} {message} [{source}]{exceptions}"
                                  ) :> Logger
let task : WebPart =
    fun ctx -> WebPart.asyncOption {
     let! ctx = GET ctx
     let! ctx = Writers.setHeader "foo" "bar" ctx
     return ctx
    }
let mimeTypes =
  Writers.defaultMimeTypesMap
    @@ (function | ".avi" -> Writers.createMimeType "video/avi" false | _ -> None)
let app =
    choose [
        GET >=> path "./hello" >=> never
        pathRegex "(.*?)\.(dll|mdb|log)$" >=> RequestErrors.FORBIDDEN "Access denied."
    ]
[<EntryPoint>]
let main argv =
    startWebServer
        { bindings = [ HttpBinding.createSimple HTTP "127.0.0.1" 8082 ]
          serverKey = Utils.Crypto.generateKey HttpRuntime.ServerKeyLength
          errorHandler = defaultErrorHandler
          listenTimeout = TimeSpan.FromMilliseconds 2000.
          cancellationToken = Async.DefaultCancellationToken
          bufferSize = 2048
          maxOps = 100
          autoGrow = true
          mimeTypesMap = mimeTypes
          homeFolder = None
          compressedFilesFolder = None
          logger = logger
          tcpServerFactory = new DefaultTcpServerFactory()
          cookieSerialiser = new BinaryFormatterSerialiser()
          tlsProvider = new DefaultTlsProvider()
          hideHeader = false
          maxContentLength = 1000000 }
        app
    0 // return an integer exit code

```fsharp

