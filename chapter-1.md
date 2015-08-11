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

Consider:

	from operator import mul
	def square(x):
	    return mul(x, x)
	square(-2)

1. The first line import a "mul(...)" function to GF (global frame) (Notice that we use function signature).
1. The second and the third lines defines a function "square(x)" which generates a function to GF.
1. The final line calls for the user-defined function "square(x)" with parameters being "-2". This calling envokes a local frame asdf


### Analogy to _Mathematica_

It is something like the "<name> $ <random number>" trick in _Mathematica_ for defining local variables, collection of which defines the local frame in "Module[]".
