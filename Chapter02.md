# Chapter 02 #

## 2.1 Class Definitions ##

Classes in Fancy are first-class values (they are instances of the
`Class` class, just as in Ruby or Smalltalk).

There's several ways to define Classes in Fancy, but the common one is
to use the built-in syntax for it. Compared to Smalltalk, where class
definitions are just message sends as well, Fancy also provides syntax
for doing so, just as Ruby and most other object-orientd programming
languages out there.

    class Person {
    }

Defines an empty class called `Person` which implicitly inherits from
`Object`, the root class of Fancy's class hierarchy.

To define a class that inherits from another class we specify the
super class after a colon, like so:

    class ImaginaryPerson : Person {
      # Person is the super class of ImaginaryPerson
      # ...
    }

## 2.2 Method definitions ##

Let's define a `name` method for `Person` which returns a `String`
representing the name of a `Person` (for now we'll just use the same
name for all instances of `Person`):

    class Person {
      def name {
        "John Doe"
      }
    }

If we create a new `Person` now and send it the `name` message, we'll
get "John Doe" as a response:

    p = Person new
    p name # => "John Doe"


### 2.2.1 Constructor method ###

Let's refactor this code a little and define a constructor that takes
a name and returns that when sent the `name` message:

    class Person {
      def initialize: name {
        @name = name
      }

      def name {
        @name
      }
    }

As you can see, constructor methods *start* with `initialize`. In our
case the constructor method just takes one argument, a name, and
stores it into the *instance variable* `@name`. Instance variables
start with `@` and *class variables* start with `@@`, just as in Ruby.

Now we can create persons and pass in their names:

    john = Person new: "John Doe"
    betty = Person new: "Betty Boop"
    john name # => "John Doe"
    betty name # => "Betty Boop"

Constructor methods like `initialize:` are just normal instance
methods, like `name`. They're called with the appropriate arguments
when calling the `Class##new:` class method. For constructors with
multiple arguments, the necessary class constructor methods are
created for you implicitly. Let me show you what I mean:

    class Person {
      def initialize: name age: age city: city {
        @name = name
        @age = age
        @city = city
      }

      def age {
        @age
      }

      def city {
        @city
      }
    }

    p = Person new: "John Doe" age: 42 city: "New York"
    p name # => "John Doe"
    p age # => 42
    p city # => "New York"

When defining a method starting with `initialize:` it will create a
corresponding class method replacing `initialize:` with `new:` and
keeping the rest of the name. E.g. for an instance method called
`initialize:age:city:` a class method `new:age:city:` is defined which
passes all the arguments on to the `initialize` method.

### 2.2.2 Slot readers & writers ###
