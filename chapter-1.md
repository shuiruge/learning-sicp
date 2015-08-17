This draft based on the lecture at [here](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html).


# Basics I

## Environment

> A name evaluates to the value associated with that name in the current environment.

This quotation recovers the reason of naming it as "environment". Indeed, *embeding* a name (not string) into an environment is to look up the temporal memory to find out what this name is binded by. A name has its meaning only when the environment embeding it is indicated.


## Evaluate and Execute

1. "Evaluate" means a map from the set combined by the set of expressions and the set of enviroments to (their indicated) values.
1. "Execute" means a map from the set of environments to itself.

> Each type of expression or statement has its own evaluation or execution procedure.




## Pure and Non-pure Function

1. Pure function is easy to understand.

1. Non-pure function can be illustrated by the following two instances:

> A nested expression of calls to print highlights the non-pure character of the function.
>
>    >>> print(print(1), print(2))
>    1
>	 2
>	 None None
>
> If you find this output to be unexpected, *draw an expression tree* to clarify why evaluating this expression produces this peculiar output.

and

>    >>> two = print(2)
>    2
>    >>> print(two)
>    None
In this instance, "two" is assigned to the output of "print(2)", which is "None". If you try
	two
it returns nothing, since "None" in Python means nothing to return. How to display the value of two, i.e. the output of "print(2)", so that we can ensure "print()" is a non-pure function as shown? It can be
	print(two)
As we have seen, it returns the output of "print(2)", which is "None"!

### Benefit of Pure Function

> Pure functions are restricted in that they cannot have side effects or change behavior over time. Imposing these restrictions yields substantial benefits. First, pure functions can be composed more reliably into compound call expressions. We can see in the non-pure function example above that print does not return a useful result when used in an operand expression. On the other hand, we have seen that functions such as max, pow and sqrt can be used effectively in nested expressions.
>
> Second, pure functions tend to be simpler to test. A list of arguments will always lead to the same return value, which can be compared to the expected return value. Testing is discussed in more detail later in this chapter.
>
> Third, ......



# Function

## Define a Function

The syntax is

> def <name>(<formal parameters>):
>     return <return expression>

The second line must be *indented*! Convention dictates that we indent with four spaces.


## Environment (Again)

### Environment Diagram

This _environment diagram_ shows the bindings of the current environment, along with the values to which names are bound.

### Frames

"Frame" is defined as:

> An environment in which an expression is evaluated consists of a sequence of frames, depicted as boxes. Each frame contains bindings, each of which associates a name with its corresponding value.
>
>There is a single global frame.

### Function Signature

"Function signature" is defined as:

> The name of the function, and its parameters.

It looks quite like Lojban, where a gismu is a collection of its name and its "slots". *Only the name of a gismu is not the gismu itself!*

The function max can take an arbitrary number of arguments. It is rendered as max(...).

Function signature is used in the construction of the environment diagram.


## Calling for User-defined Function

### Local Frame

Calling for user-defined function envokes a _local frame_. An instance to declare it is stated as follow.

> Applying a user-defined function introduces a second local frame, which is only accessible to that function. To apply a user-defined function to some arguments:
>
> Bind the arguments to the names of the function's formal parameters in a new local frame.
> Execute the body of the function in the environment that starts with this frame.
> The environment in which the body is evaluated consists of two frames: first the local frame that contains formal parameter bindings, then the global frame that contains everything else. Each instance of a function application has its own independent local frame.

### Instance:

	from operator import mul
	def square(x):
	    return mul(x, x)
	square(-2)

> 1. After executing the first import statement, only the name mul is bound in the global frame.
> 1. Next, the definition statement for the function square is executed. Notice that the entire def statement is processed in a single step. The body of a function is not executed until the function is called (not when it is defined).
> 1. Then, the square() function is called with the argument -2, and so a new frame is created with the formal parameter x bound to the value -2.
> 1. Then, the name x is looked up in the current environment, which consists of the two frames -- the global and the local associated to function square. The function mul() is embedded in the global frame. In both occurrences, x evaluates to -2, and so the square function returns 4.
>
> (The "Return value" in the square() frame is not a name binding; instead it indicates the value returned by the function call that created the frame.)

### Another instance

Consider:
	from operator import mul
	x = 1;
	def square(x):
		return mul(x, x)

Inputting
	square(-2)

returns
	4

And inputting
	x

returns
	1

instead of -2. This means the x in square(x) is the so-called _local variable_ as in _Mathematica_.

But, how can this _local variable_ be created in interpreter? Just embedding into frames?
	

### A Possible Solution
	
One possible solution I can imagine is binding local name (i.e. name of local variable) with the name of the called function. For instance, for this x in square(x) is re-named in the local frame of square(x) when it is called as
	x_INVALID-NOTATION_square 

where  "INVALID-NOTATION" denotes any notation that is invalid in naming a variable in the language.

### Analogy to _Mathematica_

It is something like the "<name>" + "$" + "<random number>" trick in _Mathematica_ for defining local variables, collection of which defines the local frame in "Module[]".


## 'Black Box' Abstraction

> We can write sum_squares without concerning ourselves with _how_ to square a number. The details of how the square is computed can be suppressed.
>
> In other words, a function definition should be able to suppress details. The users of the function may not have written the function themselves, but may have obtained it from another programmer as a "black box". *A programmer should not need to know how the function is implemented in order to use it.* The Python Library has this property. Many developers use the functions defined there, but few ever inspect their implementation.


## Guide on Naming Functions and Parameters

> 1. Function names are lowercase, with words separated by underscores. Descriptive names are encouraged.
>
> 1. Function names typically evoke operations applied to arguments by the interpreter (e.g., print, add, square) or the name of the quantity that results (e.g., max, abs, sum).
>
> 1. Parameter names are lowercase, with words separated by underscores. Single-word names are preferred.
>
> 1. Parameter names should evoke the role of the parameter in the function, not just the kind of argument that is allowed.
>
> 1. Single letter parameter names are acceptable when their role is obvious, but avoid "l" (lowercase ell), "O" (capital oh), or "I" (capital i) to avoid confusion with numerals.

### Pity

> There are many exceptions to these guidelines, even in the Python standard library. Like the vocabulary of the English language, Python has inherited words from a variety of contributors, and the result is not always consistent.

# The Art  of the Function

> Fundamentally, the qualities of good functions all reinforce the idea that functions are abstractions.
>
> 1. Each function should have exactly one job. That job should be identifiable with a short name and characterizable in a single line of text. Functions that perform multiple jobs in sequence should be divided into multiple functions.
> 1. Don't repeat yourself is a central tenet of software engineering. The so-called DRY principle states that multiple fragments of code should not describe redundant logic. Instead, that logic should be implemented once, given a name, and applied multiple times. If you find yourself copying and pasting a block of code, you have probably found an opportunity for functional abstraction.
> 1. Functions should be defined generally. Squaring is not in the Python Library precisely because it is a special case of the pow function, which raises numbers to arbitrary powers.

But how to ensure these? Python provides several features to support these efforts, stated as follow.

## "docstring"

> A function definition will often include documentation describing the function, called a docstring, which must be indented along with the function body. Docstrings are conventionally triple quoted. The first line describes the job of the function in one line. The following lines can describe arguments and clarify the behavior of the function:

	def pressure(v, t, n):
		"""Compute the pressure in pascals of an ideal gas.

        Applies the ideal gas law: http://en.wikipedia.org/wiki/Ideal_gas_law

        v -- volume of gas, in cubic meters
        t -- absolute temperature in degrees kelvin
        n -- particles of gas
        """
        k = 1.38e-23  # Boltzmann's constant
        return n * k * t / v

> When you call help with the name of a function as an argument, you see its docstring

	help(pressure)

> *When writing Python programs, include docstrings for all but the simplest functions. Remember, code is written only once, but often read many times.*

## Comment

> Comments in Python can be attached to the end of a line following the # symbol. For example, the comment Boltzmann's constant above describes k. These comments don't ever appear in Python's help, and they are ignored by the interpreter. They exist for *the author* alone.

## Default argument value

> A consequence of defining general functions is the introduction of additional arguments. Functions with many arguments can be awkward to call and difficult to read.

An instance: (*Notice the change in the docstring!!!*)
	 Boltzmann_K = 1.38e-23  # Boltzmann's constant
	 def pressure(v, t, n=6.022e23):
		 """Compute the pressure in pascals of an ideal gas.

            v -- volume of gas, in cubic meters
            t -- absolute temperature in degrees kelvin
            n -- particles of gas (default: one mole)
        """
        return n * Boltzmann_K * t / v

> In the def statement header, = does not perform assignment, but instead indicates a default value to use when the pressure function is called. So,

	>>> pressure(1, 273.15)
	2269.974834
	>>> pressure(1, 273.15, 3 * 6.022e23)
	6809.924502

