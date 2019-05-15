---
layout: post
title:  "Python Language - What's Different About It"
date:   2019-04-09 00:00:00 -0700
categories: 
---
These are some notes on things about Python that have caught my attention as I learn the core language while coming from a background in PHP, C++ and JavaScript. The items are in no particular order.

<br />
**1)  Blocks of code are defined by indentation, not by braces or keywords**

~~~~~ python
>>> m = 42
>>>   n = 13  # IndentationError: unexpected indent
~~~~~

<br />
**2)  Line breaks serve as statement terminators, usually**

However, you are allowed to put multiple statements on the same line by separating them with semicolons (although this seems to be uncommon practice).

~~~~~ python
>>> a = 17; b = 34
~~~~~

On the other hand, the following is valid syntax. Apparently, the left parenthesis causes the parser to hold its breath and expect a continuation of the statement on the next line.

~~~~~ python
>>> print(
... 'hello world')
~~~~~

<br />
**3)  Parameters can be passed either positionally or by name (aka keyword) or both**

For example, given function

~~~~~ python
def foo(color, material, cost):
~~~~~ 

foo can be called in several ways.

~~~~~ python
foo('blue', 'steel', 5000)

foo(cost = 1000, color = 'red', material = 'wood')
~~~~~

You can mix keyword params with positional params but the positional params have to be provided first.

~~~~~ python
foo('purple', cost = 2000, material = 'denim')
~~~~~

<br />
**(4)  Everything is an object, including integers and strings so there are no scalar types**

You can confirm this by using the built-in id( ) function which returns a unique id among existing objects.

~~~~~ python
>>> id(42)
14218568

>>> 'hello world'.upper()
'HELLO WORLD'
~~~~~

<br />
**(5)  There is a ternary operator, whose syntax is different from some other languages**

~~~~~ python
>>> n = 42 if want_answer else 13
~~~~~

<br />
**(6)  Additional arithmetic operators for integer division and exponentiation**

~~~~~ python
# integer division
>>> 42 // 5
8

# exponentiation
>>> 2 ** 10
1024
~~~~~

And support for large integers.

~~~~~ python
>>> 2 ** 1024
17976931348623159077293051907890247336179769789423065727343008115773267580550096313270847732240753602112011387987139335765878976881
44166224928474306394741243777678934248654852763022196012460941194530829520850057688381506823424628814739131105408272371633505106845
86298239947245938479716304835356329624224137216L
~~~~~

<br />
**(7)  Built-in functions type( ), dir( ), help( ) and id( ) are really informative**

~~~~~ python
>>> type('hello world')
<type 'str'>

>>> dir('hello world')
['__add__', '__class__', '__contains__', '__delattr__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
'__getitem__', '__getnewargs__', '__getslice__', '__gt__', '__hash__', '__init__', '__le__', '__len__', '__lt__', '__mod__',
'__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__',
'__str__', '__subclasshook__', '_formatter_field_name_split', '_formatter_parser', 'capitalize', 'center', 'count', 'decode',
'encode', 'endswith', 'expandtabs', 'find', 'format', 'index', 'isalnum', 'isalpha', 'isdigit', 'islower', 'isspace', 'istitle',
'isupper', 'join', 'ljust', 'lower', 'lstrip', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit',
'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']

>>> help(id)
Help on built-in function id in module __builtin__:

id(...)
    id(object) -> integer
    
    Return the identity of an object.  This is guaranteed to be unique among simultaneously existing objects.  (Hint: it's the object's memory address.)
~~~~~

<br />
**(8)  There are both lists [ ] and tuples ( )**

Lists [ ] are mutable, tuples ( ) are not. 

~~~~~ python
>>> t1 = (10, 20, 30)
>>> t1[0] = 17
TypeError: 'tuple' object does not support item assignment
~~~~~

In practice, tuples are often used for sequences of heterogeneous elements, while lists are often homogeneous. In other words, lists are often used the way arrays are used in other languages, while tuples can sometimes be thought of as records.

<br />
**(9)  Parentheses are used both to group expressions and to define tuples**

Consequently, to define a single-element tuple, you need to add a trailing comma so that it is treated as a tuple and not as an expression.

~~~~~ python
t1 = (42)   # t1 is an int
t2 = (42, ) # t2 is a tuple with one element
~~~~~

Note that in a tuple, trailing commas are allowed, but not required unless it's a single element tuple.

<br />
**(10)  The names of built-in types are not reserved keywords**

They are more like pre-defined identifiers and can be assigned values, which then changes their type. Beware that this might cause some confusion!

~~~~~ python
>>> type(int)
<type 'type'>

>>> int = 42
>>> type(int)
<type 'int'>
~~~~~

<br />
**(11)  Some methods counter-intuitively return the value None instead of an object**

For example, append( ) when operating on a list returns None, not the extended list.

~~~~~ python
>>> list_1 = [10, 20, 30]
>>> print(list_1.append(40))
None
~~~~~

<br />
**(12)  You can make a boolean expression that consists of multiple comparisons**

~~~~~ python
>>> n = 50
>>> 0 < n < 100
True
~~~~~

<br />
**(13)  pass statement**

An empty statement is provided by using pass, which is a no-op. This is useful to serve as the body of a function or class that is empty. Otherwise, you might get a syntax error.

~~~~~ python
>>> def foo():
>>>     pass  # empty body
~~~~~

<br />
**(14)  Mutable default values for parameters**

If the default value for a parameter is provided and the value is a mutable object, and that object is modified by the function, then the modified object becomes the new default value!

~~~~~ python
>>> def bar(seq = [3, 4, 5]):
>>>     seq.append(13)  # does an in-place append
>>>     return len(seq)

>>> print(bar())
4
>>> print(bar())
5
~~~~~

<br />
**(15) Anonymous functions are defined using the lambda keyword**

For example, to define a function that returns the square of its argument,

~~~~~ python
>>> lambda n: n ** 2
~~~~~

<br />
**(16) Iterators are heavily used and supported**

Lists and tuples have built-in iterators.

Functions such as map( ), zip( ) and filter( ) construct and return iterators that can be used, for example, to define lists.

~~~~~ python
>>> seq = [2, 3, 5, 7, 11]
>>> list( map( lambda n: n ** 2, seq ) )
[4, 9, 25, 49, 121]
~~~~~

<br />
**(17) Comprehensions - list, dict, set**

Comprehensions are a concise and elegant way to define the contents of a list, dict or set. Here are a couple of list comprehensions.

~~~~~ python
>>> [n ** 2 for n in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

>>>  [n for n in range(10) if n % 2 == 0 or n % 3 == 0]
[0, 2, 3, 4, 6, 8, 9]
~~~~~

<br />
**(18) Generator functions**

"... a generator looks like a function but behaves like an iterator" ~ from Wikipedia under the topic of _Generator (computer programming)_

If you are familiar with the concept of coroutines, Python generator functions are a kind of coroutine.

When the _yield_ statement is executed it passes back a value and determines the next point at which execution resumes.

The _return_ statement raises an exception to stop the iteration.

Here is a generator function to produce the Fibonacci sequence up to some maximum value. Calling the function returns a generator object that acts like an iterator.

~~~~~ python
def fib():
    a = b = 0
    while b < 100:
        result = a + b
        if result == 0:
            result = 1
        a = b
        b = result
        
        yield result

    return  # raises StopIeration exception

>>> for n in fib():
>>>     print(n)
1
1
2
3
5
8
13
21
34
55
89
144
~~~~~

An advantage to using a generator is that there is no need to first create the full sequence, which can mean a substantial savings in memory and time.

<br />
**(19)  Generator expressions**

Generator expressions look like list comprehensions except that parentheses are used instead of square brackets. (Note this is yet another overloaded use of parens.)

Like generator functions, using a generator expression instead of a list could result in big savings in memory and time. Another similarity is that a generator expression acts like a one-use iterator.

Here are two different ways to sum a long sequence of squares. The first uses a list comprehension. It ran for over a minute in my local Jupyter notebook before the Jupyter kernel died. The second uses a generator expression. It took several minutes but did run to completion.

~~~~~ python
>>> sum([n**2 for n in range(10**9)])  # Did not complete

>>> sum(n**2 for n in range(10**9))
333333332833333333500000000
~~~~~

<br />
**(20)  There are both static and class methods**

The difference is that class methods take the class object as a first parameter, whereas static methods do not and are like static methods in other languages.

Class methods have two common uses: first, as class factory methods that take different parameters for instantiating an instance of the class; second, for refactoring static methods into smaller helper methods.

~~~~~ python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    @classmethod
    def from_tuple(cls, coords):  # cls is Point
        return cls(coords[0], coords[1])

    @classmethod
    def from_point(cls, point):  # cls is Point
        return cls(point.x, point.y)

>>> p = Point.from_tuple((32, 47))
>>> print(p.x, p.y)
32 47

>>> q = Point.from_point(p)
>>> print(q.x, q.y)
32 47
~~~~~

<br />
**(21)  Visibility and name mangling**

Python does not have visibility keywords such as public or private.

There is a convention of starting the name of an attribute with a single underscore when the attribute is intended to be private, but the language does nothing to enforce the intention.

However, if an attribute defined in a class has a name that starts with at least two underscores and trails with no more than one underscore, then its name is mangled in that the internal name of the class is prefixed with the name of the class.

This effectively makes the attribute somewhat private since any use of the same name in a subclass results in a differently mangled internal name.

~~~~~ python
class SomeBase():
    def __init__(self):
        self.__color = 'blue'
    def get_color(self):
        return self.__color

class SomeChild(SomeBase):
    def __init__(self):
        self.__color = 'yellow'
    def get_color(self):
        return self.__color

>>> base_obj = SomeBase()
>>> print(base_obj.get_color())
blue

>>> child_obj = SomeChild()
>>> print(child_obj.get_color())
yellow
~~~~~

But the attribute is not _truly_ private because if you know how the name is mangled you can access it.

~~~~~ python
>>> print(base_obj._SomeBase__color)
blue
~~~~~

<br />
**(22)  The @property decorator**

A method that is decorated with @property syntactically acts like an attribute whose value can be read.

~~~~~ python
class Person():

    def __init__(self, age):
        self.__age = age

    @property
    def age():
        return self.__age
~~~~~

So in code outside of the class we can write,

~~~~~ python
>>> person = Person(42)
>>> print(person.age)  # like an attribute, not a method call!
42
~~~~~ 

To define a corresponding setter method, there is a special decorator. Assuming the above age( ) method we can write,

~~~~~ python
@age.setter
def age(self, new_age):
    self.__age = new_age

>>> person.age = 39
~~~~~ 

<br />
**(23)  Operator overloading**

Certain operators can be overloaded for classes. You can see a list of these by calling the dir( ) function on the class.

For example, here we overload the + operator for class Book to return a list of the titles of the two operands.

~~~~~ python
class Book():
    
    def __init__(self, title = 'No Name'):
        self._title = title
        
    def __add__(self, other):
        return [self._title, other._title]

>>> b1 = Book()
>>> b2 = Book('Hello World')

>>> print(b1 + b2)
['No Name', 'Hello World']
~~~~~

<br />
**(24)  Custom iterators**

It is possible to write a class that is a custom iterator. For example, here is one that iterates through a dictionary in the sorted order of the keys.

~~~~~ python
class DictIter():
    """Returns values in a dictionary in key-sorted order"""
    
    def __init__(self, dictionary):
        self._dict = dictionary
        self._keys = sorted(dictionary)
    
    def __iter__(self):
        return self

    def __next__(self):
        if self._keys:
            return self._dict[self._keys.pop(0)]
        raise StopIteration

>>> # Instantiate a DictIter iterator and use it.
>>> anim = {'d':'donkey', 'c':'cougar', 'b':'bear', 'a':'aardvark'}
>>> d_iter = DictIter(anim)

>>> print([animal for animal in d_iter])
['aardvark', 'bear', 'cougar', 'donkey']
~~~~~
