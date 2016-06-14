# Chapter 02 #

## 2.1 Class Definitions ##

Classes in Fancy are first-class values (they are instances of the
`Class` class, just as in Ruby or Smalltalk).

There's several ways to define Classes in Fancy, but the common one is
to use the built-in syntax for it. Compared to Smalltalk, where class
definitions are just message sends as well, Fancy also provides syntax
for doing so, just as Ruby and most other object-oriented programming
languages out there.

```fancy
class Person {
}
```

Defines an empty class called `Person` which implicitly inherits from
`Object`, the root class of Fancy's class hierarchy.

To define a class that inherits from another class we specify the
super class after a colon, like so:

```fancy
class ImaginaryPerson : Person {
  # Person is the super class of ImaginaryPerson
  # ...
}
```


## 2.2 Method definitions ##

Let's define a `name` method for `Person` which returns a `String`
representing the name of a `Person` (for now we'll just use the same
name for all instances of `Person`):

```fancy
class Person {
  def name {
    "John Doe"
  }
}
```

If we create a new `Person` now and send it the `name` message, we'll
get "John Doe" as a response:

```fancy
p = Person new
p name # => "John Doe"
```


### 2.2.1 Constructor method ###

Let's refactor this code a little and define a constructor that takes
a name and returns that when sent the `name` message:

```fancy
class Person {
  def initialize: name {
    @name = name
  }

  def name {
    @name
  }
}
```

As you can see, constructor methods *start* with `initialize`. In our
case the constructor method just takes one argument, a name, and
stores it into the *instance variable* `@name`. Instance variables
start with `@` and *class variables* start with `@@`, just as in Ruby.

Now we can create persons and pass in their names:

```fancy
john = Person new: "John Doe"
betty = Person new: "Betty Boop"
john name # => "John Doe"
betty name # => "Betty Boop"
```

Constructor methods like `initialize:` are just normal instance
methods, like `name`. They're called with the appropriate arguments
when calling the `Class##new:` class method. For constructors with
multiple arguments, the necessary class constructor methods are
created for you implicitly. Let me show you what I mean:

```fancy
class Person {
  def initialize: name age: age city: city {
    @name = name
    @age = age
    @city = city
  }

  def name {
    @name
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
```

When defining a method starting with `initialize:` it will create a
corresponding class method replacing `initialize:` with `new:` and
keeping the rest of the name. E.g. for an instance method called
`initialize:age:city:` a class method `new:age:city:` is defined which
passes all the arguments on to the `initialize` method.

### 2.2.2 Slot readers & writers ###

There's a simpler way to define so-called *getter-* and
*setter-methods* in Fancy:

```fancy
class Person {
  # define getter-methods:
  read_slots: ['name, 'age, 'city]

  # or if, you'd want them all to be writable:
  write_slots: ['name, 'age, 'city]

  # or define both getter & setter methods:
  read_write_slots: ['name, 'age, 'city]
}
```

We usually refer to *instance variables* as ***slots*** within
Fancy. It's just a different name, but they refer to the exact same
thing.

`read_slots:` defines reader methods for all the slotnames within a
given Array. `write_slots:` defines setter methods for the slotnames
in a given Array. `read_write_slots:` does both at once. These methods
are completely defined in Fancy and can be found in the `Class` class
definition in Fancy's standard library.

### 2.2.3 More syntactic sugar ###

OK, let's have a look at some more nice syntactic features Fancy
provides for common idioms in method definitions.
First, let's have a look at all the code for the `Person` class we've
written so far. We'll modify it a little to have multiple constructor
methods to emulate default arguments for the missing arguments:

```fancy
class Person {
  def initialize: name {
    initialize: name age: 42 # default is 42
  }

  def initialize: name age: age {
    initialize: name age: age city: "New York" # default is "New York"
  }

  def initialize: name age: age city: city {
    @name = name
    @age = age
    @city = city
  }

  # slot readers

  def name {
    @name
  }

  def age {
    @age
  }

  def city {
    @city
  }

  # let's also define slot writer methods:

  def name: new_name {
    @name = new_name
  }

  def age: new_age {
    @age = new_age
  }

  def city: new_city {
    @city = new_city
  }
}
```

OK, and now let's define the same stuff with some standard helper
methods and syntax sugar:

```fancy
class Person {
  read_write_slots: ['name, 'age, 'city]

  def initialize: @name age: @age (42) city: @city ("New York") {
  }
}
```

Yup, that's it. =)
What's happening here is we're defining getter & setter methods for
the slots `@name, @age` and `@city` and use Fancy's auto slot assignment
syntax in the constructor method, which sets the slot with the given
name to the value passed in as the argument. Finally, we're also using
default argument values (any expression within `(` and `)`). Note,
that you're allowed to refer to arguments passed in further left as
default arguments further right.

For example:

```fancy
def hello: name1 and: name2 (name1) {
  "Hello, #{name1} and #{name2}!" println
}

# call with both arguments:
hello: "Robert" and: "Meggie" # prints "Hello, Robert and Meggie!"

# call with one argument:
hello: "Max" # prints "Hello, Max and Max!"
```


#### The [next chapter][Chapter 3] will introduce some built-in classes with literal support commonly used in the language. ####

  [Chapter 3]: Chapter03.md
