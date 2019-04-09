---
layout: post
title:  "Learning Python - Random Notes"
date:   2019-04-08 00:00:00 -0700
categories: 
---
These are some random notes on things about Python that caught my attention as I learn the core language. The items are in no particular order.

<br />
**1)  Blocks of code are defined by indentation, not by braces or keywords**

    >>> m = 42
    >>>  n = 13  # IndentationError: unexpected indent

<br />
**2)  Line breaks serve as statement terminators, usually**

However, you are allowed to put multiple statements on the same line by separating them with semicolons (although this seems to be uncommon practice).

    >>> a = 17; b = 34

On the other hand, the following is valid syntax. It seems to be the left parenthesis that causes the parser to hold its breath and not expect a complete statement on that line.

    >>> print(
    ... 'hello world')

<br />
**3)  Parameters can be passed either positionally or by name (aka keyword) or both**

For example, given function

    def foo(color, material, cost):

foo can be called in several ways.

    foo('blue', 'steel', 5000)

    foo(cost = 1000, color = 'red', material = 'wood')

You can mix keyword params with positional params but the positional params have to be provided first.

    foo('purple', cost = 2000, material = 'denim')

<br />
**(4)  Everything is an object, including integers and strings so there are no scalar types**

You can confirm this by using the built-in id( ) function which returns a unique id among existing objects.

    >>> id(42)
    14218568

    >>> 'hello world'.upper()
    'HELLO WORLD'

<br />
**(5)  There is a ternary operator, whose syntax is different from some other languages**

    >>> n = 42 if want_answer else 13

<br />
**(6)  Additional arithmetic operators for integer division and exponentiation**

    # integer division
    >>> 42 // 5
    8

    # exponentiation
    >>> 2 ** 10
    1024

And support for large integers.

    >>> 2 ** 1024
    17976931348623159077293051907890247336179769789423065727343008115773267580550096313270847732240753602112011387987139335765878976881
    44166224928474306394741243777678934248654852763022196012460941194530829520850057688381506823424628814739131105408272371633505106845
    86298239947245938479716304835356329624224137216L

<br />
**(7)  Built-in functions type( ), dir( ), help( ) and id( ) are really informative**

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

<br />
**(8)  There are both lists [ ] and tuples ( )**

Lists [ ] are mutable, tuples ( ) are not. In practice, tuples are often used for sequences of heterogeneous elements, while lists are often homogeneous.

In other words, lists are often used the way arrays are used in other languages, while tuples can sometimes be thought of as records.

<br />
**(9)  Parentheses are used both to group expressions and to define tuples**

Consequently, to define a single-element tuple, you need to add a trailing comma so that it is treated as a tuple and not as an expression.

    t1 = (42)   # t1 is an int
    t2 = (42, ) # t2 is a tuple with one element

Note that in a tuple, trailing commas are allowed, but not required unless it's a single element tuple.

<br />
**(10)  The names of built-in types are not reserved keywords**

They are more like pre-defined identifiers and can be assigned values, which then changes their type. Beware that this might cause some confusion!

    >>> type(int)
    <type 'type'>

    >>> int = 42
    >>> type(int)
    <type 'int'>

<br />
(11)  Some methods counter-intuitively return the value None instead of an object

For example, append( ) when operating on a list returns None, not the extended list.

>>> list_1 = [10, 20, 30]
>>> print(list_1.append(40))
None


(12)  You can make a boolean expression that consists of multiple comparisons

>>> n = 50
>>> 0 < n < 100
True


(13)  pass statement

An empty statement is provided by using pass, which is a no-op. This is useful to serve as the body of a function or class that is empty.


(14)  Mutable default values for parameters

If the default value for a parameter is provided and the value is a mutable object, and that object is modified by the function, then the modified object becomes the new default value!

>>> def bar(seq = [3, 4, 5]):
>>>     seq.append(13)
>>>     return len(seq)

>>> print(bar())
4
>>> print(bar())
5


(15) Anonymous functions are defined using the lambda keyword

For example, to define a function that returns the square of its argument,

>>> lambda n: n ** 2


(16) Iterators are heavily used and supported

Lists and tuples have built-in iterators.

Functions such as map(), zip() and filter() construct and return iterators that can be used, for example, to define lists.

>>> seq = [2, 3, 5, 7, 11]
>>> list( map( lambda n: n ** 2, seq ) )
[4, 9, 25, 49, 121]


(17) Comprehensions - list, dict, set

Comprehensions are a concise and elegant way to define the contents of a list, dict or set. Here are a couple of list comprehensions.

>>> [n ** 2 for n in range(10)]
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

>>>  [n for n in range(10) if n % 2 == 0 or n % 3 == 0]
[0, 2, 3, 4, 6, 8, 9]

(18) Generator functions

"... a generator looks like a function but behaves like an iterator" ~ from Wikipedia under the topic of Generator (computer programming)

If you are familiar with the concept of coroutines, Python generator functions are a kind of coroutine.


When the yield statement is executed it passes back a value and determines the next point at which execution resumes.

The return statement raises an exception to stop the iteration.

Here is a generator function to produce the Fibonacci sequence up to some maximum value. Calling the function returns a generator object that acts like an iterator.

def fib():
    a = b = 0
    while b < 100:
        result = a + b
        if result == 0:
            result = 1
        a = b
        b = result
        
        yield result
    return

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

An advantage to using a generator is that there is no need to first create the full sequence, which can mean a substantial savings in memory and time.


(19)  Generator expressions

Generator expressions look like list comprehensions except that parentheses are used instead of square brackets. (Note this is yet another use of parens.)

Like generator functions, using a generator expression instead of a list could result in big savings in memory and time. Another similarity is that a generator expression acts like a one-use iterator.

Here are two different ways to sum a long sequence of squares. The first uses a list comprehension. It ran for over a minute in a local Jupyter notebook before the Jupyter kernel died. The second uses a generator expression. It ran to completion over several minutes and provided the answer.

>>> sum([n**2 for n in range(10**9)])  # Did not complete

>>> sum(n**2 for n in range(10**9))
333333332833333333500000000


(20)  There are both static and class methods within classes

The difference is that class methods take the class object as a first parameter, whereas static methods do not.

Class methods have two common uses: first, as class factory methods that take different parameters for instantiating an instance of the class; second, for refactoring static methods into smaller helper methods.


(21)  Visibility and name mangling

Python does not have visibility keywords such as public or private.

There is the convention of starting the name of an attribute with a single underscore. This means the attribute is intended to be private, but the language does nothing to enforce the intention.

However, if an attribute defined in a class has a name that starts with at least two underscores and trails with no more than one underscore, then its name is mangled in the sense that the internal name of the class is prefixed with the name of the class.

This effectively makes the attribute somewhat of a private one since any use of the same name in a subclass results in a differently mangled internal name.


(22)  The @property decorator

A method that is decorated with @property syntactically acts like an attribute whose value can be read. For example,

class Person():

  def __init__(self, age):
    self.__age = age

  @property
  def age():
    return self.__age

So in code outside of the class we can write,

person = Person(42)
print(person.age)

To define a corresponding setter method, there is a special decorator. Assuming the above age() method as decorated, we can write,

  @age.setter
  def age(self, new_age):
    self.__age = new_age


(23)  Operator overloading

Certain operators can be overloaded for classes. I believe you can see a list of these by calling the dir() on the class.

For example, here we overload the + operator for class Book to return a list of the titles of the two operands.

class Book():
    
    def __init__(self, title = 'No Name'):
        self._title = title
        
    def __add__(self, other):
        return [self._title, other._title]


b1 = Book()
b2 = Book('Hello World')

print(b1 + b2)    ## output:   ['No Name', 'Hello World']


(24)  Custom iterators

It is possible to write a class that is a custom iterator. For example, here is one that iterates through a dictionary in the sorted order of the keys.

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

# Instantiate a DictIter iterator and use it.
anim = {'d':'donkey', 'c':'cougar', 'b':'bear', 'a':'aardvark'}
d_iter = DictIter(anim)

print([animal for animal in d_iter])   ## output: ['aardvark', 'bear', 'cougar', 'donkey']
