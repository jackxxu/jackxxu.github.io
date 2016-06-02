---
layout: post
title:  "is Ruby's initialization method really private?"
date:   2016-06-02 12:50:36 -0400
categories: ruby
---
While chatting with a Ruby developer recently, I was told that Ruby's initialization method is private.

"Really?" I thought, "but I always see the `initialize` method being declared in the public (not under private keyword) section of a ruby class..."

This led me to check the [Rubinius implementation]. I found the following:

### 1. Even if the `initialize` method of a Class can in the private session, its base method has been declared private.

Here is how it is declared in Rubinius:

<script src="https://gist-it.appspot.com/github/rubinius/rubinius/blob/master/core/class.rb?slice=13:33&footer=minimal"></script>

https://github.com/rubinius/rubinius/blob/master/core/class.rb

This means `initialize` method is always private, unless, of course, you declare it to be public.

<script src="https://gist.github.com/jackxxu/3b826fe7f989ce218d28e0fceb6facbf.js"></script>

### 2. How the Ruby `new` method works

This piqued me my interest on how the ruby new method works. It just seems that it is kind of redundent to have two methods, `new` and `initialize` for object instantiation.

It turns out that `new` method does the generic object creation work, and leaves it to the `initialize` method to decorate it.

<script src="https://gist-it.appspot.com/github/rubinius/rubinius/blob/master/core/alpha.rb?slice=89:103&footer=minimal"></script>

in this way, you probably should don't have to override the `new` class, unless for a really good reason and you know what you are doing.

### 3. A suggestion for Ruby

The `initialize` method is made specifically for the `new` method and there is really no reason that I can think of to call `initialize` directly. To me, having the `initialize` logic as a method seems a little confusing, especially as someone may wonder if the `initialize` method can be called by another method.

This makes me wonder if it makes more sense to have the initialization logic in a macro that can't be directly called other than when the class is loaded.

<script src="https://gist.github.com/jackxxu/c5c28021e76dc610254d9b95a4f5ba5d.js"></script>

Just a thought...

[Rubinius implementation]: https://github.com/rubinius/rubinius
