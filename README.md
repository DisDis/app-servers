# Table of Contents

* [Scope](#scope)
  * [Hello World](#hello-world)
  * [Disclaimer](#disclaimer)
* [Languages](#languages)
  * [Ruby](#ruby)
  * [Python](#python)
  * [JavaScript](#javascript)
  * [Dart](#dart)
  * [Elixir](#elixir)
  * [Java](#java)
  * [C-sharp](#c-sharp)
  * [Crystal](#crystal)
  * [Nim](#nim)
  * [GO](#go)
  * [Rust](#rust)
* [Tools](#tools)
  * [Wrk](#wrk)
  * [Platform](#platform)
  * [RAM and CPU](#ram-and-cpu)
* [Benchmarks](#benchmarks)
  * [Results](#results)
  * [Puma](#puma)
  * [Gunicorn with Meinheld](#gunicorn-with-meinheld)
  * [Node Cluster](#node-cluster)
  * [Dart HttpServer](#dart-httpserver)
  * [Plug with Cowboy](#plug-with-cowboy)
  * [Jetty NIO](#jetty-nio)
  * [Kestrel](#kestrel)
  * [Crystal HTTP](#crystal-http)
  * [Asynchttpserver](#asynchttpserver)
  * [GO ServeMux](#go-servemux)
  * [Hyper](#hyper)

## Scope
The idea behind this repository is to benchmark different languages implementation of HTTP server.

### Hello World
The *application* i tested is minimal: the HTTP version of the *Hello World* example.  
This approach allows including languages i barely know, since it is pretty easy to find such implementation online.  
If you're looking for more complex examples, you will have better luck with the [TechEmpower benchmarks](https://www.techempower.com/benchmarks/).

### Disclaimer
Please do take the following numbers with a grain of salt: it is not my intention to promote one language over another basing on micro-benchmarks.  
Indeed you should never pick a language just basing on its presumed performance.

## Languages
I have filtered the languages by single runtime (i.e. Java on JVM): this way i can focus on a specific stack, keeping it updated to the last available version/APIs. 
Where possible i just relied on the standard library, but when it is not production-ready (i.e. Ruby, Python) or where the language footprint is deliberately minimal (i.e. Rust). 

### Ruby
[Ruby](https://www.ruby-lang.org/en/) 2.5.1 is installed via [rbenv](https://github.com/rbenv/rbenv).  
Ruby is a general-purpose, interpreted, dynamic programming language, focused on simplicity and productivity According to his creator Ruby is inspired by Lisp, Perl, Python and Smalltalk.  

### Python
[Python](https://www.python.org/) 3.7.0 is installed via homebrew.  
Python is a widely used high-level, general-purpose, interpreted, dynamic programming language.  
It supports several programming paradigms and can count on a broad standard library.

### JavaScript
[Node.js](https://nodejs.org/en/) version 10.9.0 is installed by official OSX package.  
Node.js is based on the V8 JavaScript engine, optimized by Google and supporting most of the new language's features.   

### Dart
[Dart](https://www.dartlang.org/) version 2.0.0 is installed via homebrew.  
Dart is an object-oriented, class defined, single inheritance language using a C-style syntax that transcompiles optionally into JavaScript.
It is part of a Google ambitious project to use a single language to program server-side, client-side and mobile-side (cross-platforms).

### Elixir
[Elixir](http://elixir-lang.org/) 1.7.3 is installed via homebrew.  
Elixir is a purely functional language that runs on the [Erlang](https://www.erlang.org/) VM.  
While preserving Erlang key-features, Elixir is strongly influenced by Ruby syntax and supports compile-time metaprogramming with macros and polymorphism.

### Java
[Java](https://www.java.com/en/) JDK 10.0.1 is installed by official OSX package.  
Java is a general-purpose language that is concurrent, class-based, object-oriented and, from version 8, supports functional paradigms.
It is based on a battle-tested virtual machine (JVM) that kept the promise "write once, run anywhere".

### C-sharp
[C-sharp](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)) (C#) 7.0 language is part of the [.NET Core]((https://www.microsoft.com/net/core#macos) 2.1.4 framework.  
C# is a simple, powerful, type-safe, object-oriented language. It inherited many features from Java, but recently added some desirable paradigms such as futures, pattern matching and deconstructions.  

### Crystal
[Crystal](http://crystal-lang.org/) 0.26.1 is installed via homebrew.  
Crystal has a syntax very close to Ruby, but brings some desirable features such as statically typing and raw metal speed, thanks to ahead of time (AOT) compilation.  

### Nim
[Nim](http://nim-lang.org/) 0.18.0 is installed via homebrew.  
Nim is an efficient, Python inspired, statically typed language that comes with an ambitious compiler aimed to produce code in C, C++, JavaScript or ObjectiveC.
Nim supports metaprogramming, functional, message passing, procedural, and object-oriented coding style.

### GO
[GO](https://golang.org/) language version 1.11 is installed by official OSX package.  
GO focuses on simplicity by intentionally lacking features considered redundant. GO takes a straight approach to parallelism, coming with built in [CSP](https://en.wikipedia.org/wiki/Communicating_sequential_processes) and green threads (go-routines).  

### Rust
[Rust](https://www.rust-lang.org/) language version 1.28.0 is installed by official package.  
According to the official site Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety.  
These bounds are granted courtesy of Rust's pretty unique ownership model enforced by the compiler.

## Tools

### Wrk
I used [wrk](https://github.com/wg/wrk) as the loading tool.  
I measured each application server six times, picking the best lap (but for VM based languages demanding longer warm-up).  
```shell
wrk -t 4 -c 100 -d30s --timeout 2000 http://0.0.0.0:9292
```

### Platform
These benchmarks are recorded on a MacBook PRO 15 mid 2015 having these specs:
* macOS High Sierra
* 2.2 GHz Intel Core i7 (4 cores)
* 16 GB 1600 MHz DDR3

### RAM and CPU
I measured RAM and CPU consumption by using macOS Activity Monitor dashboard and recording max consumption peak.  
For the languages relying on pre-forking parallelism i reported the average consumption by taking a snapshot during the stress period.

## Benchmarks

### Results
| Language                  | App Server                                        | Req./sec (local)  | RAM (MB)  | CPU (%)  |
| :------------------------ | :------------------------------------------------ | ----------------: |---------: |--------: |
| [Dart](#dart)             | [Dart HttpServer](#dart-httpserver)               |         36833.66  |    422.1  |   527.1  |
| [Nim](#nim)               | [Asynchttpserver](#asynchttpserver)               |         46263.57  |      5.7  |    99.8  |
| [Elixir](#elixir)         | [Plug with Cowboy](#plug-with-cowboy)             |         51349.23  |     42.4  |   495.1  |
| [Ruby](#ruby)             | [Puma](#puma)                                     |         56411.07  |    > 100  |   > 390  |
| [JavaScript](#javascript) | [Node Cluster](#node-cluster)                     |         88866.37  |    > 240  |   > 390  |
| [Crystal](#crystal)       | [Crystal HTTP](#crystal-http)                     |         94571.88  |      8.4  |   105.4  |
| [C-Sharp](#c-sharp)       | [Kestrel](#kestrel)                               |         96434.94  |    979.6  |   495.4  |
| [Rust](#rust)             | [Hyper](#hyper)                                   |         96847.92  |      3.7  |   497.4  |
| [GO](#go)                 | [GO ServeMux](#go-servemux)                       |         97703.19  |      7.6  |   454.1  |
| [Python](#python)         | [Gunicorn with Meinheld](#gunicorn-with-meinheld) |         98563.64  |     > 40  |   > 350  |
| [Java](#java)             | [Jetty NIO](#jetty-nio)                           |        104570.11  |    224.4  |   433.5  |

                                                                                                   
### Puma                                                                                 
I tested Ruby by using a plain [Rack](http://rack.github.io/) application served by [Puma](http://puma.io).

#### Bootstrap
```shell
puma -w 8 -t 8 --preload servers/puma_server.ru
```


### Gunicorn with Meinheld
I tested Python by using [Gunicorn](http://gunicorn.org/) spawning [Meinheld](http://meinheld.org/) workers.

#### Bootstrap
```shell
cd servers
gunicorn -w 4 -k meinheld.gmeinheld.MeinheldWorker -b :9292 gunicorn_server:app
```


### Node Cluster
I used the cluster module included into Node's standard library.

#### Bootstrap
```shell
node servers/node_server.js
```


### Dart HttpServer
I used the async HTTP server embedded into the Dart standard library.

#### Bootstrap
```shell
dart servers/dart_server.dart
```


### Plug with Cowboy
I tested Elixir by using [Plug](https://github.com/elixir-lang/plug) library that provides a [Cowboy](https://github.com/ninenines/cowboy) adapter.

#### Bootstrap
```shell
cd servers/plug_server
MIX_ENV=prod mix compile
MIX_ENV=prod mix run --no-halt
```


### Jetty NIO
I tested Java by using [Jetty](http://www.eclipse.org/jetty/) with the non blocking IO (NIO) APIs.  

#### Bootstrap
```shell
cd servers/jetty_server
javac -cp jetty-all-uber.jar HelloWorld.java
java -server -cp .:jetty-all-uber.jar HelloWorld
```


### Kestrel
To test C# i opted for [Kestrel](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/servers/kestrel): a cross-platform web server based on the libuv asynchronous I/O library.

#### Bootstrap
```shell
cd servers/kestrel_server
dotnet restore
dotnet run
```


### Crystal HTTP
I used Crystal HTTP server standard library, not supporting parallelism yet.  

#### Bootstrap
```shell
crystal build --release servers/crystal_server.cr
./crystal_server
```


### Asynchttpserver
I used the asynchttpserver standard module of Nim, not supporting parallelism yet.

#### Bootstrap
```shell
nim c -d:release servers/nim_server.nim
./servers/nim_server
```


### GO ServeMux
I used the [HTTP ServeMux](https://golang.org/pkg/net/http/) GO standard library.  

#### Bootstrap
```shell
go run servers/go_server.go
```


### Hyper
I tested Rust by using [Hyper](https://hyper.rs/), a HTTP client/server based on the [Tokio](https://tokio.rs/) toolkit.

#### Bootstrap
```shell
cd servers/hyper_server
cargo run --release
```
