---
layout: post
title:  "A Few Syntax Quirks from Ruby"
date:   2019-08-27 00:00:00 -0700
categories: 
---
Coming from an academic background in compilers and programming languages, I enjoy getting into the dark corners of a language. Recently, I have been picking up Ruby and have noticed what I consider to be syntactic quirks.

<hr width="80%" />
<br />
Method calls in Ruby allow but do not require parentheses around the arguments. From the code I've seen so far, the common convention seems to be that parentheses are almost always omitted.

But this flexibility means that spaces are sometimes significant. For instance, 

  `puts('hello', 'world')`

works as expected.

But if there is more than one argument, a space between the method name and the opening parenthesis gives a syntax error.

  `puts ('hello', 'world')`

With a single argument the space doesn't matter.

  `puts('hello')`  
  `puts ('hello')`

Although I have not verified this with the language spec, it looks like an opening parenthesis immediately after a method name is parsed as a token signifying a list of arguments, whereas the space causes the parenthesis to be parsed as an argument, that is, an expression.

Somewhat similarly, in the following call of `length`, a syntax error occurs because the `-1` is treated as an argument, but `length` takes no arguments.

  `arr = [11, 13, 17]`  
  `last = arr.length -1`

Remove the space and it works fine, although a little crowded for my tastes.

  `last = arr.length-1`

Or maybe you can make the method call more explicit by using parentheses.

  `last = arr.length() -1`

<hr width="80%" />
<br />

