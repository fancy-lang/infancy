# Chapter 03 #

## 3.1 Numbers ##

As any programming language, Fancy has number literal syntax built in.

### 3.1.1 Integers (Fixnums) ###

There are multiple ways to write a `Fixnum` (Integer value). Here's a list of possible ways to write a `Fixnum`:

* `1`, `123`, `1_000_000`, `999_888_777` etc. Underscores are ignored and used purely to be easier to read (especially large numbers)
* Binary literals: `0b10101`, `0B01010101111`
* Hexadecimal literals: `0xff`, `0xAB`, `0XAF`
* Octal literals: `0o77`, `0O13`

### 3.1.2 Floats ###

Floats only allow one way of writing them: `10.9997`, `0.123`, `1.0`

**Note:** Both Fixnums and Floats can be prefixed with a `-` for negative numbers.


## 3.2 Strings ##

Strings are what you expect them to be.

Examples:

```fancy
"Hello, World"
"Hello,\n World"

"""
This is a multi-line String
The newlines become part of the String and start and end with a triple quote,
just as in Python. They're most commonly used for docstrings, which we'll talk about
later (also copied from Python).
"""
```

Fancy also has support for Ruby-like string interpolation:

```fancy
"This is a String with a value interpolated: 3 ** 3 = #{3 ** 3}"
"Hello, #{name}, I'm #{self age} years old."
```

## 3.3 Symbols ##

Symbols are unique identifiers. They're equivalent to Ruby's Symbols.

Examples:

```fancy
'foo
'foo?
'foo:bar:
'foo_bar!?Baz!?!
'helloWorld
'hello123!?=*
```


## 3.4 Tuples ##

In contrast to Ruby and like Python, Fancy has built-in literal syntax for Tuples. Tuples are constant-sized containers with index-based access to members.

Examples:

```fancy
(1, 2) # Tuple with values 1 and 2
(1, 2, "foo")
('foo, 'bar, "baz!")
(1, (2, 3), (4, (5, 6, 7))) # nested Tuples

# Accessing elements:
(1, 2, 3) at: 2 # => 3
(1, 2, 3)[0]    # => 1
```


## 3.5 Arrays ##

One of the most used data structures are lists and arrays. In Fancy there's the `Array` class which provides index-based constant time access to dynamically resizable (growing) polymorphic containers.
Let's have a look at some `Array` examples:

```fancy
# An Array containing 5 integers (Fixnums)
[1,2,3,4,5]

# An Array of 2 numbers, 1 String and another Array
[1, 2, "An Array!", [4,5, "Yes, indeed!"]]
```

Arrays work the same way as they do, for example, in Ruby. In Fact, they're sharing the same Class as their Ruby equivalents. Anytime you're using a Fancy Array, you're also using a Ruby Array inside Rubinius. There is no wrapper in between. This is true not only for Arrays, but for all built-in types and classes where there is a Ruby equivalent available.


## 3.6 Hashes (Dictionaries, Hashmaps) ##

Hashes are unordered collections of key-value pairs with fast access through keys. They can nest arbitrarily, just as Arrays.

Examples:

```fancy
<['name => "Fancy", 'type => "Programming Language", 'created => 2010]>
```

You can of course use any other type besides Symbols as keys and values.


## 3.7 Blocks (BlockEnvironment, Closures) ##

One of the most heavily used literal values in Fancy are Blocks. They're used to implement nearly all control structures (like traditional `if`, `else`, `while` etc constructs) and are proper closures with interesting `return` semantics.

We've seen Blocks in Chapter 1 before, but for completeness, here's a list of some literal Blocks:

```fancy
{ "A Block with no argument" println }
|x| { "A Block with one argument, value: #{x}" println }
|x y| { "A Block with two arguments, values: #{x} and #{y}" println }

# Seperating arguments with comma is optional, thus this is valid as well:
|x, y| { "The sum is: #{x + y}" println }
```

There's also *Partial Blocks* which can be used for Blocks only taking
one argument:

```fancy
@{ + 1 } # is the same as: |x| { x + 1 }
# e.g.:
[1,2,3,4] map: @{+ 1} # => [2,3,4,5]
```

To `call` a Block, you send it the `call` or `call:` message and pass the arguments in as an Array:

```fancy
block = |x, y| { x + y println }
block call: [2, 3] # will print 5
```


## 3.8 Regular Expressions ##

Fancy, as Ruby or Perl, has Regular Expressions built-in to the
language. In fact, it uses the same implementation that Ruby uses on
top of Rubinius, including the same literal syntax:

```fancy
/^Hello, (.*)!$/
/^[A-Z][a-Z0-9_:]$/
```


## 3.9 Ranges ##

Fancy also has Ranges, like Ruby:

```fancy
(1..10)
(a..b)
(x .. x ** x)
```