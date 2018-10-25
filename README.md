
# Elixir: scalable, distributed and fault-tolerant applications

- Elixir: the programming language
- Language tour: fundamental things you should know
- Talking about "scalable":
	- Developer scalabality
	- Language scalability
- Talking about "distributed":
	- The BEAM VM
	- Fundamental principles (processes, transparent locality, etc)
	- Code examples
- Talking about "fault-tolerant":
	- "Let it Crash" mentality
	- Supervisors
	- Supervision trees
	- Code examples
- Leveraging Erlang
- Notable tools and frameworks
- Conclusion

## Elixir: The Programming Language

![Elixir Logo](https://elixir-lang.org/images/logo/logo.png)

Welcome! Today, I will be discussing the Elixir language: a functional programming language designed on top of the BEAM VM, built for massive scalability and low-latency, soft real-time applications.

Elixir is a great fit for applications that are meant to be scalable, distributed and fault-tolerant. You might have heard of it, and you might not.
Today, we're gonna hop into a new realm of software development. One that you could consider is a hidden gem and that I think has a lot to teach us about application design.

We're gonna dissect what "scalable", "distributed" and "fault-tolerance" means in Elixir. Let's go!

## Language tour: fundamentals

Elixir is a functional programming language designed to build low-latency, fault-tolerant and soft real-time applications.

We're gonna have a sip of this (heh), mainly taking a look at how Elixir code looks like, and it's fundamental features.

- Functional programming!
- Basic types
- Basic operators
- Pattern matching (highly important!)
- Data structures (lists, keyword lists, maps, structs, mapsets)
- Modules and functions
- Processes

## Talking about "scalable"

If you have ever worked with anything even remotely serious, you probably have heard of the term "scalability". At first, we can think of scalable code, which means we can take an application for a spin under high load and have it at least not break in our face.

But that's not the only aspect of scalability. Sure, "code speed" matters, and nobody wants an application that drags and takes 20s to respond to requests (I'm looking at you, VOSS-4UC).

We also want to take a look at developer scalability. Mainly:

- How much time can we train people to work on the system
- How maintainable an application is in terms of code

And we'll look at both from an Elixir perspective.

### Training

I'm not gonna lie here. Elixir is one of the programming languages we'll be seeing for the next few years, alongside Rust and Go.

The time taken to get someone that knows e.g. Java and teach them Go is ridiculous. I speak that from experience. I would say that for the average developer, it would take a minimum of two weeks. Max.

However, Go has one advantage: it is familiar. It has C-like syntax (once you get used to a better syntax, actually), Go can be Object Oriented (to some degree), Go also has a few functional tricks up its sleeve for the functional folks.

Elixir is on the same realm of Rust: the learning curve is a little steeper. Although both languages have new concepts to teach you, and trust me, they **are** worth learning, it takes a little bit more time to actually get your hands dirty and start writing meaningful code.

I would say that for a experienced Java programmer that has never done one single line of functional programming in his/her life, and didn't even had learned about it beforehand, would take about 1 month of training. And I say this based in absolutely nothing, so take it with a grain of salt. YMMV.

### Elixir and application maintainability

We're gonna compare solutions for a given problem in C and Elixir.

[Link here.](https://medium.com/@cameronp/functional-programming-is-not-weird-you-just-need-some-new-patterns-7a9bf9dc2f77)

So, as we can see, we have code that is declarative and straightforward. Sure, it takes some time to digest and spit out these patterns, but when you do actually get to that point, your applications start to get more maintainable and less error-prone.

## Talking about "distributed"

We have been tossing around the term BEAM VM. But what does it mean? What does it do? What was it designed for? By who? What does it eat? We'll explore a BEAM VM in its natural habitat by travelling to the past.

### Ericsson, 1986

The year is 1986. Telephone systems were raging, and engineers were thinking of ways to actually write telephone switches that were... well, better.

The requirements:
- One misbehaving switch **can not** kill the entire switch. So that means: fault-tolerance.
- Updating a system **can not** stop people from actually making phone calls. So that means: rolling, hot code upgrades.
- The system needs to be extremely scalable. Adding more hardware needs to be easy-peasy. So that means: distribution **and** discovery of new nodes.

The engineering team at Ericsson hacked away at a language that could meet those requirements. Turns out that everything that needs to be massively distributed needs immutability, so they already started with a functional mindset.

They came up with the "Ericsson Language", or shorter: Erlang.

In 1988, they had proven the language. Writing telephone switches was a breeze.

On the developer side. The Erlang language was still really slow. They needed a 40x improvement in order to be able to write switches suitable to a production environment. Thus, in 1992, the BEAM VM work started.

In 1998, the new AXD301 switch was announced by Ericsson. It had over a million lines of Erlang code, and astounding **nine 9's** of availability. This meant that for every year, the system would experience around 36 **milliseconds** of downtime.

Let that sink in.

### But what about Elixir?

The Elixir language is based on top of all of this. It leverages the existent Erlang ecosystem. Not only the virtual machine, but it is tightly coupled to Erlang, and can actually refer to existing Erlang code without any run-time cost at all!

### But we are not writing telephone systems!!!

True.

But the web today is... complex. More and more pictures of cats need to be transmitted at absurd speeds over the globe, in real-time. How can I drink my morning coffee without browsing [/r/aww](https://reddit.com/r/aww) and getting the latest fluffy pics of kittens the world has never seen?

More and more the web gets distributed. To multiple machines. To multiple datacenters, even. How can we use Elixir and leverage Erlang to distribute our code? And more importantly, **how hard** is it?

Spoiler alert: a breeze. We covered processes. It's time we take them to the next level.

## Talking about "fault-tolerant"

There is one last aspect that we need to cover: fault-tolerance. The ability to be able to recover from unexpected events, also known as errors.

First, let's take a look on how we would deal with errors in different mainstream programming languages.

- C: Errors are globally set using a macro, usually with a given code. Programmer needs to be aware of different codes to be able to handle failure. Not transparent and not enforced by the compiler.
- Java: Exceptions are the idiomatic approach. Code that can fail throws exceptions, and the handling of those exceptions is usually made on the top level. Compiler enforced.
- Go: Functions that can go wrong either return a value, or an error, in a special kind of tuple. The flow is explicit: from top to bottom. Leads to boilerplate very quickly. Error handling is enforced at all levels, which makes you think about errors as they happen but the delegation mechanism is poor.

Each of these approaches are **defensive**:  they deal with errors as they happen. They program themselves in the face of errors, either taking an Plan A approach for a "happy path", or a Plan {B, C, D, ...} approaches for "sad paths".

When dealing with errors in Elixir (and subsequently Erlang), we have one motto: "Let it crash".

### Errors are inevitable
![Resultado de imagem para let it go](http://cdn.revistadonna.clicrbs.com.br/wp-content/uploads/2016/05/Frozens-Queen-Elsa-009.jpg)

When we're talking production, all sorts of funky things can happen. Your rug can (and will) be pulled in ways you can't (and won't) always foreseen.

Networks get split-brained. Computers and datacenters crash and burn in ways we can never expect to happen. Memory gets corrupted, kernels get panicky and users often use your code in ways that you couldn't figure out or think ahead of time.

Instead of trying to guess all of that, which can lead to madness and Cthulhu-like codebases, let's just... let it go. Let it crash. But let it crash responsably.

### Supervisors

In OTP, Erlang's framework for distributed and fault-tolerant applications, we have a concept of a Supervisor. A Supervisor is a piece of code who has your processes on a check list. Every time one of them fails, whatever the fail may be, the Supervisor knows how to restart it in a way that is guaranteed to work.

Supervisors are really smart, and accept a different number of restart strategies for every use case.

Interestingly enough, Supervisors can also supervise other Supervisors, leading to what we call a "supervision tree".

![Resultado de imagem para supervision tree](https://learnyousomeerlang.com/static/img/sup-tree.png)

In the example graph, whenever a `Worker` crashes, the parent `Supervisor` is responsible for restarting it. If the `Supervisor` itself crashes, the parent `Supervisor` restarts it, which in turn also restarts the `Worker`s.

Let's hop into an example.

## Leveraging Erlang

Erlang has a rich ecosystem that we can use (almost) for free in Elixir! That includes:

- A distributed key-value storage called ETS
- A distributed relational database called Mnesia
- Every Erlang library out there

You can call Erlang code from Elixir code without any performance penalty. The reason for that is the way Elixir is compiled down to the BEAM bytecode, which I won't be covering in this KSS.

Some of the most famous Erlang libraries that we use in projects are:

- Cowboy: highly available HTTP server
- Poolboy: generic resource pool manager
- Hackney and IBrowse: HTTP requests

Sometimes, you'll find Elixir wrappers for some of these libraries, but only when it makes sense. Most of the time, using the Erlang counterpart is totally acceptable. The rule of thumb is that the Elixir wrapper must at least provide extra functionality on top of the existing Erlang library.

## Notable Tools

### Phoenix

An awesome web framework, inspired by Rails. It's a breeze to work with. It's so easy we'll build a small web application in the next 5 mins.

### Ecto

Ecto is... amazing. It's primary concern is about mapping data to Elixir structs, and the primary source for that data is usually a relational database table. So you could say Ecto is a ORM. Or, maybe... a FRM?

### ExUnit

ExUnit is that type of library that makes you really proud of the programming language it is built for. Contains nice features we'll discuss in one of our examples, including a very special one called doctests.

### ExDoc

ExDoc generates documentation from special markers in your Elixir code. Even the Elixir documentation is written using ExDoc.. We'll go over an example quickly.

### Distillery

Distillery is the library that makes Erlang releases available to Elixir projects. In Erlang, releases are a binary format for your application that contain everything it needs to run out of the box, including the possibility to attach remote consoles to a running application.

### Nerves

Nerves is a framework made for running Elixir in embedded devices. It uses Buildroot to compile a lean 12MB Linux that boots directly to your Elixir application. You get all the goodness in your embedded devices, including distribution inside a network and fault-tolerance. It's an amazing project that already powers a few industrial devices!

## Conclusion

![Resultado de imagem para CHOO CHOO MOTHERFUCKER](https://i.kym-cdn.com/photos/images/newsfeed/000/544/719/a6c.png)

> "The world is changing. It's time we change too" - Adrian Toomes, Spider-Man: Homecoming

Building scalable, distributed and fault-tolerant systems is not an easy task. It's daunting.
With the correct tools, building systems like this can be fun. Challenging, still, but definitely fun.

I'm not saying we should jump on the hype train and _CHOO CHOO MOTHERF*****_ into Elixir for building the next Broadsoft. But there's valuable lessons buried deep into the Erlang ecosystem, specially OTP.

Knowledge never hurts. And programming languages that actually teach you something are definitely worth learning.
