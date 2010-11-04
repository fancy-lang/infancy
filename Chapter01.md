# Chapter 01 #
## What is Fancy ? ##

Fancy is a dynamic, concurrent, pure object-oriented general purpose
programming language inspired by
[Smalltalk](http://en.wikipedia.org/wiki/Smalltalk),
[Ruby](http://en.wikipedia.org/wiki/Ruby_programming_language), and
[Erlang](http://en.wikipedia.org/wiki/Erlang_programming_language).
It runs on [Rubinius](http://www.rubini.us).

The goal is to create a language implementation that is easy to
understand and improve, even for people new implementing programming
languages.

Fancy's compiler is currently written in Ruby, while all of Fancy's
standard library is written in Fancy itself. It's a good starting
point if you want to get a better feel for the language and its
built-in classes and methods, once you've mastered the fundamental
semantics and syntax.
You can view the standard library classes on GitHub
[here](https://github.com/bakkdoor/fancy/blob/master/lib/).

OK, enough said, let's get started with Fancy's basic concepts.

## Basic concepts ##

Fancy is heavily inspired by
[Smalltalk](http://en.wikipedia.org/wiki/Smalltalk), a pure
object-oriented, message passing dynamic programming language,
developed at [XEROX PARC](http://www.parc.com/) in the 1970s and
1980s.

The core idea of Smalltalk (and thus Fancy) is the concept of
**message sending** (also called *message passing*). Fancy code
interacts by sending messages to objects and getting responses while
doing so. You can think of methods as message handlers for incoming
messages on objects. In Fancy, as in Smalltalk and Ruby, every value
is an Object. There is no distinction between so-called *value types*
and *reference types*. In Fancy, every value is a *reference type*,
meaning each argument in a message send in Fancy is passed around as a
reference (*call by reference semantics*).

### Message Sends ###

In Fancy nearly all operations are done via *message sends*. While
Fancy does have syntax for class & method definitions, importing files
and so on, most of them are just syntactic sugar for equivalent message
sends to objects.

Let's have a look at how this looks syntactically:

    object message_name: arg

Is a message send to `object` with a message called `message_name:`
and an argument `arg`. As in Smalltalk (or Objective-C, which is
inspired by Smalltalk), messages are keyword-based and so are
methods. So if you have multiple arguments in a message send, each
argument is preceeded by a keyword (usually describing the argument).

Multiple arguments:

    object foo: arg1 bar: arg2 baz: arg3

The above shows a message send with 3 arguments for the `foo:bar:baz:`
message. If `object` or its class define a method with the same name,
it will get executed with the given arguments. If not, it will look
for a method called `unkown_message:params:` within `object`s
inheritance chain (as with Ruby's `method_missing`).

Lets look at a more real-world example:

    [1,2,3] each: |x| {
      x println
    }

This peace of code shows a message send to a literal Array consisting
of the three number values `1`, `2` and `3`. The `each:` method
expects a `Block` object (or actually, something `callable` - it needs
to implement the `call` and `call:` methods). It will `call` the
argument given to it with each element in the Array.

The `|x| { x println }` thing is a Block literal. Blocks are just
normal objects but with a built-in syntax (like strings, numbers and
many more). As in Ruby, anything between the pipes (`|`) are arguments
to the block and the code between the curly braces is the body of the
block. Blocks are used as anonymous methods (also called *lambda
functions*) and *closures* within Fancy. They are first-class values
in the language, like any other Object, and can be passed around to any
method. In Fancy all control structures are implemented with
blocks. But we'll get to that in a minute.

First off, let's fully explain that code above. As mentioned, the
`each:` method takes something that implements `call:` and passes it
each element in the array in turn. You can think of `each:` being used
as an **iterator**. In terms of the language, `each:` is just a method
like any other though.

The `println` send to `x` causes it to be displayed on
`stdout`. Here's the implementation of `println`:

    def println {
      Console println: (self to_s)
    }

You can find the code in *lib/object.fy* within Fancy's root source
directory or just [click here](https://github.com/bakkdoor/fancy/blob/master/lib/object.fy#L14) to view it on Github.
