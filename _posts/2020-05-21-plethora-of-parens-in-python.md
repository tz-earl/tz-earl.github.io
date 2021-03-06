---
layout: post
title:  "There is a Plethora of Parentheses in Python"
date:   2020-05-22 00:00:00 -0700
categories: 
---
As I get more deeply into the ins and outs of core Python, it seems that parentheses are syntactically used quite a bit, more than I have encountered in other languages.

Here are the uses I know about so far.

<br />
**(1) Group sub-expressions**

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
**(2) Provide arguments when making calls to functions or methods or other callables**

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
**(3) List the formal parameters for function and method definitions**

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
**(4) List superclasses in class definitions**

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
**(5) Tuple literals**

~~~~~
t = ('I', 'am', 'a', 'tuple')
~~~~~

Syntax note: to disambiguate an expression containing a single value from a tuple that contains a single value, add a trailing comma to the tuple.

~~~~~
i_am_an_integer = (42)
i_am_a_tuple = (42, )
~~~~~
<br />
**(6) Tuple unpacking in assignments**

Although it seems seldom used in practice, the use of parentheses to enclose multiple targets on the left-hand side of an assignment is allowed and is described as _tuple unpacking_.

This also applies to the implicit assignments in _for_ loops.

~~~~~
 a, b, c  = [42, 99, 17]
(a, b, c) = [42, 99, 17]

(a, b, c) = range(0, 3)  # Iterator on the right-hand side.

for (x, y) in [(1, 1), (2, 3), (5, 8)]:
    print(x, y)
~~~~~
<br />
**(7) Generator expressions**

Finally, parentheses are used when defining generator expressions.

~~~~~
d, e, f, g =  (n for n in range(0, 4))
~~~~~

When passing a generator as an argument to a callable and it is the only argument, then the parentheses for the expression can be omitted.

~~~~
def print_iter(it):
    for item in it:
        print(item)

print_iter( n for n in range(2, 5) )
print_iter((n for n in range(2, 5)))
~~~~
