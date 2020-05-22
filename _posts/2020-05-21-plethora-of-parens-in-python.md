---
layout: post
title:  "There's a Plethora of Parentheses in Python"
date:   2020-02-21 00:00:00 -0700
categories: 
---
As I get more deeply into the ins and outs of core Python, it seems that parentheses are syntactically used quite a bit, more than I have encountered in other languages.

Here are the uses I know about so far.

<br />
**(1) To group sub-expressions**

Creating sub-expressions within a larger expression, probably originating in math. In software there are two different reasons for these parentheses. First, to override operator precedence so that operators are evaluated in a different order. Second, to make the expression more readable and easier for humans to parse.

~~~~~
a = (b + c) * d   # Apply the addition before the multiplication
p = (q and r) or (s and t)  # Reinforces that 'and' has higher precedence
~~~~~

Syntax note: an open parenthesis enables the matching closing parenthesis to be placed on a different line without the need to escape the line endings with the backslash \ character. This can be convenient for breaking up long, complex expressions.

~~~~~
w = (x
      + y 
      + z)
~~~~~
<br />
**(2) To provide arguments when making calls to functions or methods or other callables**

Callables include classes as well as instances of any class that defines the `__call__()` method.

~~~~~
length = len([42, 17, 55])  # Call a built-in function
pi = int("314159")  # Call a class to instantiate an instance

class double_down:
    def __call__(self, num):
        return num * 2

dd = double_down()  # Create an instance
dd(1024)  # Call the instance
~~~~~
<br />
**(3) To list the formal parameters for function and method definitions**

Again, an open parenthesis enables its closing parethesis to be on a subsequent line.

~~~~~
# This def will parse fine
def some_func(
     param_1,
     param_2
    ):
    pass

# However, this def results in an invalid syntax error
def some_func
    (
     param_1,
     param_2
    ):
    pass
~~~~~
<br />
**(4) To list superclasses in class definitions**

The list of superclasses can be empty, in which case the parentheses can be omitted. But the class `object` is implied to be the superclass of every class, either directly or indirectly.

~~~~~
import collections

class MyClass(collections.UserDict):
    pass

class EmptySuperclassList():
    pass

class NoParentheses:
    pass
~~~~~
<br />
**(5) To define tuple literals**

~~~~~
t = ('I', 'am', 'a', 'tuple')
~~~~~

Syntax note: to disambiguate an expression containing a single value from a tuple that contains a single value, add a trailing comma to the tuple.

~~~~~
i_am_an_integer = (42)
i_am_a_tuple = (42, )
~~~~~
