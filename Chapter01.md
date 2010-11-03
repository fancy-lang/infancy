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
interacts by sending messages to objects and getting reponses while
doing so. You can think of methods as message handlers for incoming
messages on objects. In Fancy, as in Smalltalk and Ruby, every value
is an Object. There is no distinction between so-called *value types*
and *reference types*. In Fancy, every value is a *reference type*,
meaning each argument in a message send in Fancy is passed around as a
reference (*call by reference semantics*).
