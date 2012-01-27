# Chapter 04 #

## 4.1 More Blocks ##

Blocks play a vital role in Fancy. They're used for many different
things that all have one thing in common: Allow seperation of concerns
in a natural way and providing the possibility to write code close to
your problem domain. This means writing DSLs in Fancy is a natural and
easy thing to do, due to the keyword-based message syntax.

## 4.2 Partial Blocks ##

Fancy has the concept of Partial Blocks.
Partial Blocks behave like regular Blocks but have their argument
ommitted. They always only take one argument which is implicit and
that argument gets prefixed into any message send within the Block as
its receiver. They allow Ruby-style simple DSLs while not relying on
`instance_eval`, as many Ruby DSLs do, thus preserving things like the
binding of self within the Block. Examples for libraries using
`instance_eval` heavily are Sinatra and parts of Ruby on Rails.

One good example is Fancy's built-in
[HTML Generator Class](https://github.com/bakkdoor/fancy/blob/master/lib/html.fy)
which can be used for writing Webapps, for example. I wrote a simple
url-shortener webapp with Sinatra for Fancy. See
[here](https://github.com/bakkdoor/shortefy).
You'll also find a link to a Screencast there.

Let's have a look at what I mean.

### 4.2.1 Without Partial Blocks ###

```fancy
require: "html"

html = HTML new: |h| {
  h html: |h| {
    h head: |h| {
      h title: "Using HTML Class"
    }
    h body: |h| {
      h div: { id: "header" } with: |h| {
        h h1: { class: "title" } with: "My Title"
      }
      h div: { id: "footer" } with: |h| {
        h h3: "The End."
      }
    }
  }
}
```

### 4.2.2 And the same code with Patial Blocks ###

```fancy
require: "html"

html2 = HTML new: @{
  html: @{
    head: @{
      title: "Using HTML Class"
    }
    body: @{
      div: { id: "header" } with: @{
        h1: { class: "title" } with: "My Title"
      }
      div: { id: "footer" } with: @{
        h3: "The End."
      }
    }
  }
}
```

Both code snippets will produce this output:

```html
<html>
  <head>
    <title>
      Using HTML Class
    </title>
  </head>
  <body>
    <div id="header">
      <h1 class="title">
        My Title
      </h1>
    </div>
    <div id="footer">
      <h3>
        The End.
      </h3>
    </div>
  </body>
</html>
```

Since Partial Blocks preserve the meaning of self, you can use the
HTML generator class for your view code and don't need to use a
seperate template language, as all instance variables will be bound
nicely, depending on where you put that code.