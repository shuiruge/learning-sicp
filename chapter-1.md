This draft based on the lecture at [here](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html).

## Environment

> A name evaluates to the value associated with that name in the current environment.

This quotation recovers the reason of naming it as "environment". Indeed, _embeding_ a name (not string) into an environment is to look up the temporal memory to find out what this name is binded by. A name has its meaning only when the environment embeding it is indicated.


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
