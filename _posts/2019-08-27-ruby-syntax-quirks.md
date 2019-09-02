---
layout: post
title:  "A Few Syntax Quirks in Ruby"
date:   2019-08-27 00:00:00 -0700
categories: 
---
Coming from an academic background in compilers and programming languages, I enjoy getting into the obscure corners of a language. Recently, I have been learning Ruby and have noticed what I consider to be a couple of syntactic quirks.

<hr width="80%" />
<br />
**Spaces**

Method calls in Ruby allow but do not require parentheses around the arguments. From the code I've seen so far, the common convention seems to be that parentheses are always omitted in simple expressions.

But this flexibility means that spaces are sometimes significant. For instance, 

  `puts('hello', 'world')`

works as expected.

But if there is more than one argument, a space between the method name and the opening parenthesis gives a syntax error.

  `puts ('hello', 'world')`  
  `>> syntax error, unexpected ')', expecting end-of-input`  

With a single argument the space doesn't matter.

  `puts('hello')`  
  `puts ('hello')`

Although I have not verified with the language spec, it looks like an opening parenthesis immediately after a method name is parsed as a token signifying a list of arguments. However, the space causes the parenthesis to be parsed as an argument, that is, it's an expression.

Somewhat similarly, in the following call of `length`, a syntax error occurs because the `-1` is treated as an argument, but `length` takes no arguments.

  `arr = [11, 13, 17]`  
  `last = arr.length -1`  
  `>> ArgumentError (wrong number of arguments (given 1, expected 0))`

Remove the space and it works fine, although it looks a little crowded.

  `last = arr.length-1`

Or you can make the method call more explicit by using parentheses.

  `last = arr.length() -1`

<hr width="80%" />
<br />
**Commas**

Commas mean different things but can be a little confusing because Ruby often allows tokens (syntactic elements) to be omitted.

a) Commas can indicate the elements of an array. For example,

  `m = 5, 7`

is the same as

  `m = [5, 7]`

b) Commas can indicate multiple arguments to a method call. For example,

  `puts k, m, n`

is the same as

  `puts(k, m, n)`

c) On the left side of an assignment, a comma indicates multiple variables to be assigned. For example,

  `k, m = 5, 7`

results in both variables being assigned values.

d) But then how is the following parsed?

  `j = 4, m = 5, 6`

j is assigned an array of three elements. m is assigned 5. In other words, it is equivalent to

  `j = [4, (m = 5), 6]`

This seems to imply that the assignment operator has both lower and higher precedence than the comma depending on the context. An alternate parsing that seems more consistent would be

  `j = [4, m = [5, 6] ]`

<hr width="80%" />
<br />
**Line breaks**

Finally, line breaks are significant because they act as tokens. For example,

  `if n == 43 then m = 2; k = 17 else m = 3; k = 19 end`

is usually written without `then` and without the semicolons as

~~~~~
if n == 43
  m = 2
  k = 17
else
  m = 3
  k = 19
end
~~~~~

But then, sometimes a line break is **not** allowed. For example,

~~~~~
3.times do |i|
  puts(i)
end
~~~~~

is correct, but the following is not.

~~~~~
3.times
do |i|
  puts(i)
end
>> syntax error, unexpected keyword_do_block, expecting end-of-input
~~~~~
