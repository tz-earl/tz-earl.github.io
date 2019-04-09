---
layout: post
title:  "Examples of How to Use Kramdown / Markdown"
date:   2019-03-27 00:00:00 -0700
categories: 
---

<em> This is italicized because the embedded html tag is recognized </em>

But what if I am writing about html tags? \<em> this is not italicized 
because I backslash escaped the opening bracket of the html tags \</em>

A code block, with each line of code starting with four spaces. This uses an inline-attribute-list to indicate python code.

    a = some_value; # Like a very long line of code that contains a line break in the markup.
    b = [42, 43, 44];
{:.language-python}

Another code block that is fenced using kramdown syntax consisting of a line of tilde characters.  
The code block is indicated as being in python.
~~~~~ python
c = sqrt(5)
d = [3, 4, 5]
~~~~~

Here is a regular paragraph, but with a line break because of two trailing spaces following this colon:  
Second line of the same paragraph.

Header 1 using equal signs
==========================

<h1> Header 1 using HTML tag </h1>

# Header 1 using leading \# sign 

Header 2 using hyphens
-------------------
<h2> Header 2 using html tag </h2>

## Header 2 using leading \#\# signs

### Header 3 using three leading \# signs

> Blockquote created by using a leading \> character:  
Tell me, what do you plan to do  
With your one wild and precious life?

- - - - - 
<br />
* Unordered list item 1

* List item 2
* List item 3
  * Sublist item 1
  * Sublist item 2


1. Ordered list item 1
^
1. Ordered list item 2
   1. Sublist item 1
   2. Sublist item 2

- - - -
<br />
1. Here is an automatic link:  <http://www.nytimes.com/>

2. And an inline link:  [New York Times](http://www.nytimes.com/)

3. Finally, a reference link, like so:  [New York Times][nytimes]  
This allows defining a url in one place and being able to re-use it.

[nytimes]: http://www.nytimes.com/

- - - - -

An image here. Note that I had to use the localhost url to do this, not a file path!!

![The Art of Being Apart](http://localhost:4000/images/the-art-of-being-apart.jpg "The Art of Being Apart, by Brian Rea"){:height="300px" width="450px"}

- - - - -

Some text that is **strongly emphasized** by using two asterisks. Typically, it is **bolded**.

Some text that is *lightly emphasized* by using one asterisk. Typically, it is _italicized_.

Some text that uses **both strong and _light_ emphasis**, one embedded in the other

And here is some inline code:  `lambda x: x**3`
