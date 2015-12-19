This draft based on the lecture at [here](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/objects.html).

I will drop the quotation notation, which makes this note not that clear. In fact, most of this content is quoted from the lecture.

## Data Abstraction

### Review of Functional Abstraction

Functional Astraction is *abstraction of pattern (of definitions of functions)*, which, with its motivation, is summaried in the note on chapter 1 (the final section), re-written as follow.

> The important thing is that *do not to write the same thing twice!* These words directly lead to _abstraction (of pattern)_ in programming. To illustrate this, a concise (too concise to be re-stated) instance is shown in the [lecture](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#functions-as-arguments)!!
> 
> First-class status setting of Python provides vast tools for establishing this abstraction (of pattern)!

### "Object"

"Object" represents information, but also *behave* like the abstract concepts they represent. When an object is printed, it knows how to spell itself out as letters and numerals. If an object is composed of parts, it knows how to reveal those parts on demand. Objects are both information and processes, bundled together to represent the properties, interactions, and behaviors of complex things.

#### Table as an Object

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

#### Attribute

Objects have _attributes_, which are named values that are part of the object:
	
	<object>.<attribute>
	
Unlike the names that we have considered so far, these attribute names are not available in the general environment. Instead, attribute names are particular to the object instance preceding the dot.
	
	>>> today.year
	2012

#### Method

Function-valued attribute is called _method_. By implementation, methods are functions that compute their results from *both their arguments and their object*. For example, The `strftime` method ("string format of time") of `today` takes a single argument that specifies how to display a date (e.g., %A means that the day of the week should be spelled out in full).

	>>> today.strftime('%A, %B %d')
	'Monday, September 10'

By bundling behavior and information together, this Python object offers us a convincing, self-contained abstraction of a date.


#### Comments

- This dot notation looks redundant. Instead of saying `today.year`, we can say `year(today)`; instead of saying `today.strftime('%A, %B %d')`, we can say `strftime('%A, %B %d')(today)`.
 
- This looks like the `Upvalue` in _Mathematica_. Like
	
		year[today]^=2012;
	
  and once 

		today = date[2012, 9, 10]

  is evaluated, a list of up-values bounded to `today` is generated, including the `year`. But, calling `strftime[yesterday]` returns nothing.
  
- Can `Upvalue` be defined in another way, general for other language? Consider the previous instance,
		
		def year(day)
			if (day == today)
				return ...
				
  But, this defines the function `year` which can manipulate *only for `day == today`*. But, what for yesterday? Definition
		
		def year(day)
			if (day == yesterday)
				return ...
		
  will re-define the function `year`, calling `year(today)`, then, will return nothing!
  
  *So, only with the aid of function, `Upvalue` is hard to define in other language.*
	

- *There is no `Upvalue` out of _Mathematica_, so, as a substitution, for playing the same thing, object is called for!!*
  
- *Remind of the "abstraction of repeated pattern" (high-level abstraction), I wonder if this abstract appears at here. That is, is there any repeated pattern in manipulating data??*

### Function V.S. Object

Function and object stand for two aspects of all abstractions.

Sometimes process plays the role. In this case, if one process appears repeatedly, by the spirit "never construct the same thing twice", we shall bound a name to it, so that by calling this bounded name is the process called conveniently. This is, as remind, the basic level functional abstraction (remind that higher level functional abstraction relates to the "abstraction of repeated pattern").

Sometimes data plays the role -- data with values, also with functions in figuring out by putting it into the function as argument what it will return (in mma this is up-value). In this case, if one data appears repeatedly, by the spirit "never construct the same thing twice", we shall bound a name to it, so that by calling this bounded name is the data called conveniently. This is the abstraction of data.


### Data Type

