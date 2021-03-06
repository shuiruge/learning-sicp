This draft based on the lecture at [here](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/objects.html).

I will drop the quotation notation, which makes this note not that clear. In fact, most of this content is quoted from the lecture.


## Review of Functional Abstraction: Abstraction of Pattern

Functional Astraction is *abstraction of pattern (of definitions of functions)*, which, with its motivation, is summaried in the note on chapter 1 (the final section), re-written as follow.

> The important thing is that *do not to write the same thing twice!* These words directly lead to _abstraction (of pattern)_ in programming. To illustrate this, a concise (too concise to be re-stated) instance is shown in the [lecture](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#functions-as-arguments)!!
> 
> First-class status setting of Python provides vast tools for establishing this abstraction (of pattern)!

### How-to

How to gain such abstraction in practice? There are two ways. The first is the hard way: write out all functions, then pick out the common pattern they share, and then write out a new version basing on the functional abstraction. The second way is basing on experience, which heavily bases on the first way.

*So, there is no easy way to do such abstraction.*

## "Object"

"Object" represents information, but also *behave* like the abstract concepts they represent. When an object is printed, it knows how to spell itself out as letters and numerals. If an object is composed of parts, it knows how to reveal those parts on demand. Objects are both information and processes, bundled together to represent the properties, interactions, and behaviors of complex things.

### Table as an Object

Consider a specific table called "T", it has properties (attributes), such as color, made-by, and so on.

### An Instance

To illustrate what object really means, instance is shown as follow.

A date is a kind of simple object.

     >>> from datetime import date

The name date is bound to a _class_. A class represents a class of objects. Individual dates are called _instances_ of that class, and they can be _constructed_ by calling the class as a function on arguments that characterize the instance, as
    
     >>> today = date(2012, 9, 10)

While `today` was constructed from primitive numbers, it behaves like a date. For instance, subtracting it from another date will give a time difference, which we can display as a line of text by calling `str`. 
     
     >>> str(date(2012, 11, 30) - today)
     '81 days, 0:00:00'

### Attribute

Objects have _attributes_, which are named values that are part of the object:
	
	<object>.<attribute>
	
Unlike the names that we have considered so far, these attribute names are not available in the general environment. Instead, attribute names are particular to the object instance preceding the dot.
	
	>>> today.year
	2012

### Method

Function-valued attribute is called _method_. By implementation, methods are functions that compute their results from *both their arguments and their object*. For example, The `strftime` method ("string format of time") of `today` takes a single argument that specifies how to display a date (e.g., %A means that the day of the week should be spelled out in full).

	>>> today.strftime('%A, %B %d')
	'Monday, September 10'

By bundling behavior and information together, this Python object offers us a convincing, self-contained abstraction of a date.


### Why Object? Comparing with Up-value

This looks like the `Upvalue` in _Mathematica_. Like
	
	year[today]^=2012;
	
and once 
	
	today = date[2012, 9, 10]
	
is evaluated, a list of up-values bounded to `today` is generated, including the `year`. But, calling `strftime[yesterday]` returns nothing.
  
Can `Upvalue` be defined in another way, general for other language? Consider the previous instance,
	
	def year(day)
	    if (day == today)
			return ...
				
But, this defines the function `year` which can manipulate *only for `day == today`*. But, what for yesterday? Definition
		
	def year(day)
		if (day == yesterday)
			return ...
	
will re-define the function `year`, calling `year(today)`, then, will return nothing!

*So, only with the aid of function, `Upvalue` is hard to define in other language. So, as a substitution, for playing the same thing, object is called for!!*


### Function V.S. Object

Function and object stand for two aspects of all abstractions.

Sometimes process plays the role. In this case, if one process appears repeatedly, by the spirit "never construct the same thing twice", we shall bound a name to it, so that by calling this bounded name is the process called conveniently. This is, as remind, the basic level functional abstraction (remind that higher level functional abstraction relates to the "abstraction of repeated pattern").

Sometimes data plays the role -- data with values, also with functions in figuring out by putting it into the function as argument what it will return (in mma this is up-value). In this case, if one data appears repeatedly, by the spirit "never construct the same thing twice", we shall bound a name to it, so that by calling this bounded name is the data called conveniently. This is the abstraction of data.


### Data Type

So far, the types of objects we have encountered are: numbers, functions, Booleans, and now dates. We also briefly encountered sets and strings in Chapter 1, but we will need to study those in more depth. There are many other kinds of objects --- sounds, images, locations, data connections, etc.

Function `type(argument)` is used to inspect the type of some `argument`. The same operator acting on arguments with different types behaves differently. Such as `+` operator on `int` and on `float` types.


## Data Abstraction

Motivation and definition of data abstraction are stated firstly, then followed an instance, and a summary in the end.

### Motivation and Definition

As we consider the wide set of things in the world that we would like to represent in our programs, we find that most of them have compound structure. A date has a year, a month, and a day; a geographic position has latitude and longitude coordinates. To represent positions, we would like our programming language to have the capacity to couple together a latitude and longitude to form a pair, a *compound data* value that our programs can manipulate as a single conceptual unit, but which also has two parts that can be considered individually.

The use of compound data enables us to increase the modularity of our programs. If we can manipulate geographic positions directly as objects in their own right, then we can separate the part of our program that deals with values per se from the details of how those values may be represented. The general technique of isolating the parts of a program that deal with how data are represented from the parts of a program that deal with how those data are manipulated is a powerful design methodology called _data abstraction_.

#### Comment

By this motivation, we find the difference between object and simple up-value, the later has no given structure and specific ways of manipulating its "components".

#### Analogy to Black-box Abstraction

Data abstraction is similar in character to functional black-box abstraction (not the abstraction of pattern and the spirit therein). When we create a functional abstraction, the details of how a function is implemented can be suppressed. We then make an abstraction that separates the way the function is used from the details of how the function is implemented. Analogously, data abstraction is a methodology that enables us to isolate how a compound data object is used from the details of how it is constructed.

To illustrate this black-box abstraction of data, and how it is related to that of function, consider an instance on how to design a set of functions for manipulating rational numbers.

### Instance: Manipulating Rational Numbers

By that trick in chapter 1, suppose we *have had* these functions, without defining:

- `rational(n, d)` returns the rational number with numerator `n` and denominator `d`.

- `numer(x)` returns the numerator of the rational number `x`.

- `denom(x)` returns the denominator of the rational number `x`.

The first is constructor, and the left selectors, as the names reflect the meanings.

Then, even without these definitions, by them can define the addition, multiplication, and equality of two rational numbers:
	
	>>> def add_rationals(x, y):
	        nx, dx = numer(x), denom(x)
			ny, dy = numer(y), denom(y)
			return rational(nx * dy + ny * dx, dx * dy)

	>>> def mul_rationals(x, y):
		    return rational(numer(x) * numer(y), denom(x) * denom(y))
	
    >>> def eq_rationals(x, y):
		    return numer(x) * denom(y) == numer(y) * denom(x)
	
The next is to define the supposed three functions. To do so, construction of data is related. We employ pair.

#### Pair (Tuple)

A pair (in Python called _tuple_), like
	
	>>> pair = (1, 2)
	
To manipulate its components, Python has syntax (*Caution that index starts at 0!*)
	
	>>> pair[0]
	1
	>>> pair[1]
	2
	
By tuple define the three functions:
	
	>>> def rational(n, d):
	        return (n, d)
	
	>>> def numer(x):
		    return getitem(x, 0)
	
	>>> def denom(x):
		    return getitem(x, 1)
	
A function for printing rational numbers completes our implementation of this abstract data type.
	
	>>> def rational_to_string(x):
		    """Return a string 'n/d' for numerator n and denominator d."""
	        return '{0}/{1}'.format(numer(x), denom(x))
	
Our rational number implementation does not reduce rational numbers automatically. We can remedy this by changing rational. If we have a function for computing the greatest common denominator of two integers, we can use it to reduce the numerator and the denominator to lowest terms before constructing the pair. As with many useful tools, such a function already exists in the Python Library.

	>>> from fractions import gcd
	>>> def rational(n, d):
		    g = gcd(n, d)
		    return (n//g, d//g)
	
(The double slash operator, `//`, expresses integer division, which rounds down the fractional part of the result of division.)


### Instead of Employing Tuple, An Instance

Here is an instance of constructing pair, thus constructing the constructor and selectors, without employing tuple.

- If a pair p was constructed from values x and y, then getitem_pair(p, 0) returns x, and getitem_pair(p, 1) returns y.

We can implement functions pair and getitem_pair that fulfill this description just as well as a tuple.
	
	>>> def pair(x, y):
		    """Return a function that behaves like a two-element tuple."""
			def dispatch(m):
				if m == 0:
					return x
				elif m == 1:
					return y
	        return dispatch
	
	>>> def getitem_pair(p, i):
		    """Return the element at index i of pair p."""
			return p(i)
	

### Summary

Black-box abstraction makes us manipulate rational numbers without even knowing what tuple is; and all we need are the three functions, i.e., one constructor, two selectors and one printer (to string, the "standard" output, as API). This abstraction of data bases on the black-box abstraction of function.

By other types, instead of by tuple, can also define the constructor, selectors, and the printer. But, as being black-box, using which type is irrelevant to further programming basing on this three kinds of function.

One advantage is that they makes programs much easier to maintain and to modify. The fewer functions that depend on a particular representation, the fewer changes are required when one wants to change that representation.


## Sequence

Abstraction of sequence will be declared first, then follows instances of this abstraction, and then end with a DIY sequence that implement the same abstraction.

### Sequence Abstraction

A sequence is *not* a particular abstract data type, but instead a collection of behaviors that different types share. That is, there are many kinds of sequences, but they all share certain properties. In particular,

- Length. A sequence has a finite length.

- Element selection. A sequence has an element corresponding to any non-negative integer index less than its length, starting at 0 for the first element.

### Instances of the Abstraction

#### Tuple

Tuples can have arbitrary length; and they exhibit the two principal behaviors of the sequence abstraction: length and element selection.

For instance
	
	>>> digits = (1, 8, 2, 8)
	>>> len(digits)
	4
	>>> digits[3]
	8
Additionally, tuples can be added together and multiplied by integers. For tuples, addition and multiplication do not add or multiply elements, but instead combine and replicate the tuples themselves.

For instance
	
	>>> (2, 7) + digits
	(2, 7, 1, 8, 2, 8)
	>>> digits * 2
	(1, 8, 2, 8, 1, 8, 2, 8)
	
#### Mapping

Introducing mapping like mma's `Map[]` would be convenient. In Python, it is `map(func, seq)`. Remark that the result of map is an object that is *not* itself a sequence, but can be converted into a sequence by calling tuple, the constructor function for tuples.



For instance,

	>>> alternates = (-1, 2, -3, 4, -5)
	>>> map(abs, alternates)
	<map object at 0xb6f8a82c>
	>>> tuple(map(abs, alternates))
	(1, 2, 3, 4, 5)
	
It is natural that `map` returns an object instead of a tuple, since a sequence object may not be *represented* as a tuple!

##### Comment: Tensor Analogy

It looks that an object is like a tensor, while a tuple is the tensor under a representation, or say, under a system of coordinates. For a tensor, only abstractly writing is valid, without any component and functional form, unless a representation is attached to it. But, the components and functional forms change if the representation changes.

Like the previous instance, in the final line, the `tuple` function convert an abstract sequence object to a tuple-representation.

#### Multiple Assignment by Tuple

As we have seen,
	
	>>> a, b = 1, 2;
	>>> a
	1
	>>> b
	2
	
##### How Is It Done in Python?

Python actually uses tuples to represent multiple values separated by commas. This is called _tuple packing_ (or say, parenthesizing).
	
	>>> digits = 1, 8, 2, 8
	>>> digits
	(1, 8, 2, 8)
	
Using a tuple to assign to multiple names is called, as one might expect, _tuple unpacking_ (or say, um-parenthesizing).

	>>> d0, d1, d2, d3 = digits
	>>> d2
	2

Multiple assignment is just the combination of tuple packing and unpacking.


### Sequence Iteration

#### Syntax

The `for` in Python establish sequence iteration. Its syntax is

	for <name> in <expression>:
	    <suite>

#### Explanation
	
A `for` statement is executed by the following procedure:

- Evaluate the header `<expression>`, which must yield an iterable value (such as sequence).
- For each element value in that sequence, in order:
  - Bind `<name>` to that value in the local environment that surrounds `for`.
  - Execute the `<suite>`.
  
An important consequence of this evaluation procedure is that `<name>` will be bound to the last element of the sequence after the for statement is executed.
  
(The general definition of the term "iterable" appears in the section on iterators in Chapter 4.)

#### For Instance
	
	>>> def count(s, value):
		    """Count the number of occurrences of value in sequence s."""
			total = 0
			for elem in s:
				if elem == value:
					total = total + 1
	        return total
	>>> count(digits, 8)
	2
	
### Range

#### Syntax

`range(beg_int, end_int)` generates a sequence of integers run from `beg_int` to `end_int - 1` (*not* `end-int`!).

`range(end_int)` generates a sequence of integers run from `0` to `end_int - 1`.

#### For instance

	>>> range(1, 10)  # Includes 1, but not 10
	range(1, 10)

Calling the `tuple` constructor on a range will create a tuple with the same elements as the range, so that the elements can be easily inspected.

	>>> tuple(range(5, 8))
	(5, 6, 7)

#### Usage

It is quite useful in constructing `for` statement. For instance

	>>> total = 0
	>>> for k in range(5, 8):
		    total = total + k
	>>> total
	18

A common convention is to use a single underscore character `_` for the name in the for header if the name is unused in the suite:

	>>> for _ in range(3):
		    print('Go Bears!')
		
	Go Bears!
	Go Bears!
	Go Bears!

Note that an underscore is just another name in the environment as far as the interpreter is concerned, but has a *conventional* meaning among programmers that indicates the name will not appear in any expressions.

### Membership

Python provides functions behave like mma's `MemberQ`. The functions are `in` and `not in`.

#### Syntax

	<element> in <sequence>
	
	<element> not in <sequence>
	
which return Boolean values `True` or `False`.

#### Instance

	>>> digits
	(1, 8, 2, 8)
	>>> 2 in digits
	True
	>>> 1828 not in digits
	True

### Slicing

Slicing picks out some elements of a sequence but in a range to construct a "smaller" sequence. Such as picking out the "smaller" sequence `(1, 2)` in `(1, 2, 3)`.

#### Syntax
	
	<sequence> [ <beginning index> : <length> ]
	
#### Instance
	
	>>> digits[0:2]
	(1, 8)
	>>> digits[1:]
	(8, 2, 8)
	

### Nested Tuple

A tuple can be an element of anther tuple. This ability is called _a closure property_ of the tuple data type.

The usage of nested pairs is as follow.

### Recursive Sequence

For instance, tuple `(1, 2, 3, 4)` can be re-written by the nested tuple, as
	
	(1, (2, (3, (4, None))))
	
The first element of each pair is an element in the list, while the second is a pair that represents the rest of the list. The second element of the final pair is `None`, which indicates that the list has ended.

The reason is the recursion: any non-empty sequence can be decomposed into

- its first element, and
- the rest of the sequence.

And the rest of a sequence is itself a (possibly empty) sequence. We call this view of sequences recursive.

#### Constructor and Selectors

Since our list representation is recursive, we will call it an `rlist` in our implementation, so as not to confuse it with the built-in `list` type in Python.

A recursive list can be constructed from a first element and the rest of the list. The value None represents an empty recursive list. So, the constructor (and a constant) is

	>>> empty_rlist = None
	>>> def rlist(first, rest):
		    """Construct a recursive list from its first element and the rest."""
			return (first, rest)
			
And the selectors are
	
	>>> def first(s):
		    """Return the first element of a recursive list s."""
			return s[0]
	
	>>> def rest(s):
		    """Return the rest of the elements of a recursive list s."""
			return s[1]

The single behavior condition for a recursive list is that, like a pair, its constructor and selectors are inverse functions. Precisely,

- if a recursive list `s` was constructed from element `f` and list `r`, then `first(s)` returns `f`, and `rest(s)` returns `r`.


#### Instance

	>>> counts = rlist(1, rlist(2, rlist(3, rlist(4, empty_rlist))))
	>>> first(counts)
	1
	>>> rest(counts)
	(2, (3, (4, None)))
	
To archive the data abstraction, that is, to define length and elements selection. They are respectively defined as
	
	>>> def len_rlist(s):
	        """Return the length of recursive list s."""
			length = 0
			while s != empty_rlist:
				s, length = rest(s), length + 1
			return length

	>>> def getitem_rlist(s, i):
	        """Return the element at index i of recursive list s."""
			while i > 0:
				s, i = rest(s), i - 1
			return first(s)
	
Such as,
	
	>>> len_rlist(counts)
	4
	>>> getitem_rlist(counts, 1)  # The second item has index 1
	2
	
#### Recursive Definitions of Length and of Elements Selector

Both `len_rlist` and `getitem_rlist` are iterative. Recursive can they be too, as
	
	>>> def len_rlist_recursive(s):
	        """Return the length of a recursive list s."""
			if s == empty_rlist:
				return 0
				return 1 + len_rlist_recursive(rest(s))
	
	>>> def getitem_rlist_recursive(s, i):
			"""Return the element at index i of recursive list s."""
			if i == 0:
				return first(s)
				return getitem_rlist_recursive(rest(s), i - 1)
	
The same,

	>>> len_rlist_recursive(counts)
	4
	>>> getitem_rlist_recursive(counts, 1)
	2

##### Using Recursive Sequence

Recursive lists can be manipulated using both iteration and recursion. In Chapter 3, however, we will see more complicated examples of recursive data structures that will require recursion to manipulate easily.

