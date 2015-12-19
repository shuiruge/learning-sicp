This draft based on the lecture at [here](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html).

I will drop the quotation notation, which makes this note not that clear. In fact, most of this content is quotated from the lecture.


## Basics I

### Environment

The name of a newly met conception shall envoke imagination in almost everyone's mind a daily conception that is an analogy as perfect as possible. This name "environment" does so. Indeed, environment generally includes two essences:
1. bindings by which values are assigned to names, stored in memory;
1. the state of interpreter, directing the next behavior of CPU.

#### Define of Environment

Summing up, *environment means the state of your computer,* which is the environment (daily conception) the programme is embedded.


### Evaluate and Execute

1. "Evaluate" means a map from the set combined by the set of expressions and the set of enviroments to (their indicated) values.
1. "Execute" means a map from the set of environments to itself.

> Each type of expression or statement has its own evaluation or execution procedure.




### Pure and Non-pure Function

1. Pure function is easy to understand.

1. Non-pure function can be illustrated by the following two instances:

> A nested expression of calls to print highlights the non-pure character of the function.

    >>> print(print(1), print(2))
	1
	2
	None None

> If you find this output to be unexpected, *draw an expression tree* to clarify why evaluating this expression produces this peculiar output.

and

    >>> two = print(2)
    2
    >>> print(two)
    None

In this instance, "two" is assigned to the output of "print(2)", which is "None". If you try

	two

it returns nothing, since "None" in Python means nothing to return. How to display the value of two, i.e. the output of "print(2)", so that we can ensure "print()" is a non-pure function as shown? It can be
	print(two)
As we have seen, it returns the output of "print(2)", which is "None"!

#### Benefit of Pure Function

> Pure functions are restricted in that they cannot have side effects or change behavior over time. Imposing these restrictions yields substantial benefits. First, pure functions can be composed more reliably into compound call expressions. We can see in the non-pure function example above that print does not return a useful result when used in an operand expression. On the other hand, we have seen that functions such as max, pow and sqrt can be used effectively in nested expressions.
>
> Second, pure functions tend to be simpler to test. A list of arguments will always lead to the same return value, which can be compared to the expected return value. Testing is discussed in more detail later in this chapter.
>
> Third, ......



## Function

### Define a Function

The syntax is

	def <name>(<formal parameters>):
		return <return expression>

The second line must be *indented*! Convention dictates that we indent with four spaces.


### Environment (Again)

#### Environment Diagram

This _environment diagram_ shows the bindings of the current environment, along with the values to which names are bound.

#### Frames

"Frame" is defined as:

> An environment in which an expression is evaluated consists of a sequence of frames, depicted as boxes. Each frame contains bindings, each of which associates a name with its corresponding value.
>
>There is a single global frame.

#### Function Signature

"Function signature" is defined as:

> The name of the function, and its parameters.

It looks quite like Lojban, where a gismu is a collection of its name and its "slots". *Only the name of a gismu is not the gismu itself!*

The function max can take an arbitrary number of arguments. It is rendered as max(...).

Function signature is used in the construction of the environment diagram.


### Calling for User-defined Function

#### Local Frame

Calling for user-defined function envokes a _local frame_. An instance to declare it is stated as follow.

> Applying a user-defined function introduces a second local frame, which is only accessible to that function. To apply a user-defined function to some arguments:
>
> Bind the arguments to the names of the function's formal parameters in a new local frame.
> Execute the body of the function in the environment that starts with this frame.
> The environment in which the body is evaluated consists of two frames: first the local frame that contains formal parameter bindings, then the global frame that contains everything else. Each instance of a function application has its own independent local frame.

#### Instance:

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

#### Another instance

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
	

#### A Possible Solution
	
One possible solution I can imagine is binding local name (i.e. name of local variable) with the name of the called function. For instance, for this x in square(x) is re-named in the local frame of square(x) when it is called as
	x_INVALID-NOTATION_square 

where  "INVALID-NOTATION" denotes any notation that is invalid in naming a variable in the language.

#### Analogy to _Mathematica_

It is something like the "<name>" + "$" + "<random number>" trick in _Mathematica_ for defining local variables, collection of which defines the local frame in "Module[]".


### 'Black Box' Abstraction

> We can write sum_squares without concerning ourselves with _how_ to square a number. The details of how the square is computed can be suppressed.
>
> In other words, a function definition should be able to suppress details. The users of the function may not have written the function themselves, but may have obtained it from another programmer as a "black box". *A programmer should not need to know how the function is implemented in order to use it.* The Python Library has this property. Many developers use the functions defined there, but few ever inspect their implementation.


### Guide on Naming Functions and Parameters

> 1. Function names are lowercase, with words separated by underscores. Descriptive names are encouraged.
>
> 1. Function names typically evoke operations applied to arguments by the interpreter (e.g., print, add, square) or the name of the quantity that results (e.g., max, abs, sum).
>
> 1. Parameter names are lowercase, with words separated by underscores. Single-word names are preferred.
>
> 1. Parameter names should evoke the role of the parameter in the function, not just the kind of argument that is allowed.
>
> 1. Single letter parameter names are acceptable when their role is obvious, but avoid "l" (lowercase ell), "O" (capital oh), or "I" (capital i) to avoid confusion with numerals.

#### Pity

> There are many exceptions to these guidelines, even in the Python standard library. Like the vocabulary of the English language, Python has inherited words from a variety of contributors, and the result is not always consistent.

## The Art  of the Function

> Fundamentally, the qualities of good functions all reinforce the idea that functions are abstractions.
>
> 1. Each function should have exactly one job. That job should be identifiable with a short name and characterizable in a single line of text. Functions that perform multiple jobs in sequence should be divided into multiple functions.
> 1. Don't repeat yourself is a central tenet of software engineering. The so-called DRY principle states that multiple fragments of code should not describe redundant logic. Instead, that logic should be implemented once, given a name, and applied multiple times. If you find yourself copying and pasting a block of code, you have probably found an opportunity for functional abstraction.
> 1. Functions should be defined generally. Squaring is not in the Python Library precisely because it is a special case of the pow function, which raises numbers to arbitrary powers.

But how to ensure these? Python provides several features to support these efforts, stated as follow.

### "docstring"

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

### Comment

> Comments in Python can be attached to the end of a line following the # symbol. For example, the comment Boltzmann's constant above describes k. These comments don't ever appear in Python's help, and they are ignored by the interpreter. They exist for *the author* alone.

### Default argument value

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


## [Control Flow](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#control)


### Statements

(Sec. 1.5.1 + 1.5.2.)

> We can also understand multi-line programs now.
> 
> * To execute a sequence of statements, execute the first statement. If that statement does not redirect control, then proceed to execute the rest of the sequence of statements, if any remain.
>
> This definition exposes the essential structure of a recursively defined sequence: a sequence can be decomposed into its first element and the rest of its elements. The "rest" of a sequence of statements is itself a sequence of statements! Thus, we can recursively apply this execution rule. This view of sequences as recursive data structures will appear again in later chapters.
>
> The important consequence of this rule is that statements are executed in order, but later statements may never be reached, because of redirected control.


### Local Variable

(Sec. 1.5.3.)

> Whenever a user-defined function is applied, the sequence of clauses in the suite of its definition is executed in a local environment. A return statement redirects control: the process of function application terminates whenever the first return statement is executed, and the value of the return expression is the returned value of the function being applied.
>
> Thus, assignment statements can now appear within a function body.

	def percent_difference(x, y):
		""" Returns the absolute difference between
			two quantities as a percentage of the first.
		"""
	    difference = abs(x-y)
	    return 100 * difference / x
	result = percent_difference(40, 50)

> The effect of an assignment statement is to bind a name to a value in the first frame of the current environment. As a consequence, assignment statements within a function body cannot affect the global frame.


### Conditional Statements

(Sec. 1.5.4.)

	if <expression>:
		<suite>
	elif <expression>:
		<suite>
	else:
		<suite>

> When executing a conditional statement, each clause is considered in order. The computational process of executing a conditional clause follows.
>
> 1. Evaluate the header's expression.
> 1. If it is a true value, execute the suite. Then, skip over all subsequent clauses in the conditional statement.
>If the else clause is reached (which only happens if all if and elif expressions evaluate to false values), its suite is executed.

### Iteration

A while clause contains a header expression followed by a suite:

	while <expression>:
		<suite>

To execute a while clause:

1. Evaluate the header's expression.
1. If it is a true value, execute the suite, then return to step 1.


### Testing

There are at two ways of testings for now:
1. Assertion;
1. Docstring.

#### Assertion

	>>> assert fib(8) == 13, 'The 8th Fibonacci number should be 13'

When the expression being asserted evaluates to a true value, executing an assert statement has no effect. When it is a false value, assert causes an error that halts execution.

Or, an "assertion function" can be constructed:

	def fib_test():
        assert fib(2) == 1, 'The 2nd Fibonacci number should be 1'
        assert fib(3) == 1, 'The 3rd Fibonacci number should be 1'
        assert fib(50) == 7778742049, 'Error at the 50th Fibonacci number'

When writing Python in files, rather than directly into the interpreter, tests are typically written in the same file or a neighboring file with the suffix _test.py.


#### Docstring

Python provides a convenient method for placing simple tests directly in the docstring of a function. The first line of a docstring should contain a one-line description of the function, followed by a blank line. A detailed description of arguments and behavior may follow.

	def sum_naturals(n):
        """Return the sum of the first n natural numbers.

        >>> sum_naturals(10)
        55
        >>> sum_naturals(100)
        5050
        """
        total, k = 0, 1
        while k <= n:
            total, k = total + k, k + 1
        return total

For tesing, run:
	
	>>> from doctest import run_docstring_examples
	>>> run_docstring_examples(sum_naturals, globals(), True)
		Finding tests in NoName
		Trying:
			sum_naturals(10)
		Expecting:
			55
		ok
		Trying:
			sum_naturals(100)
		Expecting:
			5050
		ok

When the return value of a function does not match the expected result, the run_docstring_examples function will report this problem as a test failure.

Or, just run in command-line:

	python3 -m doctest <python_source_file>

It is even good practice to write some tests before you implement, in order to have some example inputs and outputs in your mind.


## Summary 1

For now:
1. The basic syntax of Python on function defining have been shown, including
   1. def
   1. documentation
   1. if-else
   1. while
   1. doctest & assertion
1. The important conception "environment" has been declared: It means the state of your computer.
1. Abstraction: A repetitious pattern (for now, it is values) shall be abstracted out, by, such as, defining a function. After assigning a name to it, we then just need to call this name again and again.

In the next, we will learn higher-order function.


## Higher-Order Function

To express certain general patterns as named concepts, we will need to construct functions that can accept other functions as arguments or return functions as values. Functions that manipulate functions are called _higher-order functions_.

A wonderful instance is shown in the lecture ([here] (http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#functions-as-arguments) is the link). It is concise enough that any re-statement may be foolish.


### General Function

*Repetition is the power of computer.* So, we shall keep function defined general as possible as we can, so that an user-defined function can be applied here and there, with so many times. This is what we call the abstraction of function.

#### Instance: Golden-ratio

First,

	def improve(update, isclose, guess=1):
        while not isclose(guess):
            guess = update(guess)
        return guess

*This function "improve" represents a general algorithm, by which many tasks, such as Newton's method of solving formula, finding pi, finding square of a number, can be done!!*

For this specific instance, we just need to fill in the parameters "update" and "isclose".

While, "isclose" can also be general:

	def near(x, f, g):
        return approx_eq(f(x), g(x))

	def approx_eq(x, y, tolerance=1e-3):
        return abs(x - y) < tolerance

These are general definition of (the word) "near". For our instance, "isclose" shall be constructed from the definition of words, as:

	def square_near_successor(guess):
        return near(guess, square, successor)

where
	def square(x):
	    return x * x

	def successor(x):
	    return x + 1

So, it is only the parameter "update" that is specific for our instance. By mathematical rule, it is defined as:

	def golden_update(guess):
        return 1/guess + 1

Finally, golden_ratio can be gained by calling:
	
	>>> improve(golden_update, square_near_successor)
	1.6180371352785146

#### Testing

Mathematically, there is another formula for golden_ratio, which is

	phi = 1/2 + pow(5, 1/2)/2

It can be used as a testing for our previous definition of functions:

	def near_test():
        assert near(phi, square, successor), 'phi * phi is not near phi + 1'

	def improve_test():def improve_test():
        approx_phi = improve(golden_update, square_near_successor)
        assert approx_eq(phi, approx_phi), 'phi differs from its approximation'

### How Words are Defined and Lojban

(Please skip this section!)

In this section, functions are tried to be viewed as words. We try to illustrate how words are defined. This finally lead us to Lojban.

This instance also illustrates how words, even in natural language, are defined. In computer, functions (words) are defined in a general and abstract way, such as the "near". But, abstraction also brings clearness! (As it is so in the definition of word "symmetry"!)

For simplicity, focus on function "near" and word "near" in English.

	def near(x, f, g):
        return approx_eq(f(x), g(x))

	def approx_eq(x, y, tolerance=1e-3):
        return abs(x - y) < tolerance

We say "approximately equal" of A and B in English means more than this definition. Such as saying A's taste and B's taste are approximately equal. Taste does not belong to field of reals, on which "abs()" is defined, on which "approx_eq()" is defined. But, if taste can be numerical, all is right.

Also, in English, we can say two places are near, instead of two real numbers. But if GPS is employed, position of place can be numerical. Then all is right.

But, this numerical property is itself inessential! Indeed, in _Mathematica_, all is replacement. So, in _Mathematica_, function "near()" can be defined empirically. We can define whether "Alice" and "Bob" is near or not (remind that the return of "near()" is boolean). But, this definition is not abstracted, since it is an instance! Must a definition of word in English be abstracted?? 

To be Continued!


## Closure

The conception of environment is recursive. For instance, consider locally defined function:

	def sqrt(x):
        def sqrt_update(guess):
            return average(guess, x/guess)
        def sqrt_close(guess):
            return approx_eq(square(guess), x)
        return improve(sqrt_update, sqrt_close)

Calling sqrt(256) will evoke a frame of sqrt() where the x is bound to 256. Within this frame, function sqrt\_update() and sqrt\_close() are defined. So, the frame of sqrt() looks like a global for the frames of sqrt\_update() and of sqrt\_close(). This is what the "recursive" means.

This nested design of function definition will happen if, such as in the instance of sqrt(), sqrt\_update() and sqrt\_close() are specifically for constructing the sqrt(), _without any other place to be used._

Locally defined functions are often called _closure_.


## Function as Returned "Value"

We have met the (pure) function that returns a number as its returned "value". Now, we try to study the (pure) function that returns a function as its returned "value".

*How is this returned function represented? In Python, it is represented by its name.*

#### Consider instance:

We want to compose two functions:

	def square(x):
	    return x * x
	
	def successor(x):
	    return x + 1
	
As what we have studied, composition can be as
	
	def compose(x, f, g):
	    return f(g(x))
	
	def add_one_and_square(x):
		return compose(x, square, successor)

	result = add_one_and_square(12)

But, by "Function as Returned 'Value'", it can also be (Caution that there is a leap!):
	
	def compose1(f, g):
	    def h(x):
	        return f(g(x))
	    return h
	
	add_one_and_square = compose1(square, successor)

	result = add_one_and_square(12)

This instance illustates what we have said. The "compose1(f,g)" returns a function, i.e. the (anonymous) function "h(.)"; and, in the "return" sentence, this function is represented by its name, as the "return h" (instead of "return h(x)") shows.

So, we realize that, in Python, the name of a function can represents all materials of the function, which shall originally expressed by its "function signature".

As an instance, "curring" in the next section can makes you understand more on this.


### Curring

#### What & Why?

We can use higher-order functions to convert a function that takes multiple arguments into a chain of functions that each take a single argument. More specifically, given a function f(x, y), we can define a function g such that g(x)(y) is equivalent to f(x, y). Here, g is a higher-order function that takes in a single argument x and returns another function that takes in a single argument y. This transformation is called currying.

As an example, we can define a curried version of the pow function:

	>>> def curried_pow(ground):
		    def anonym(power):
				return pow(ground, power)
			return anonym
	>>> curried_pow(2)(3)
	8

After inputting a "ground", it returns a function with argument x which represents the "pow(2, x)".

Some programming languages, such as Haskell, only allow functions that take a single argument, so the programmer must curry all multi-argument procedures.

#### Automate Curring

We can define functions to automate currying. This needs abstraction. *Before any abstraction, several instances shall be shown up!* So, re-consider the previous instance:

	def curried_pow(ground):
        def anonym(power):
            return pow(ground, power)
        return anonym

It curries pow(). Replacing to general notation of argument:

	pow --> func
	ground --> arg1
	power --> arg2
	anonym --> anonym_func

we get the re-writing

	def curry_func(arg1):
        def anonym_func(arg2):
            return func(arg1, arg2)
        return anonym_func


This algorithm curries any func to curry_func. Now we define an exacuting function by which, after inputting a func, a curry\_func is automatically defined. So, it shall be written as

	def curry_2_arg_func(func):
		def curry_func(arg1):
			def anonym_func(arg2):
				return func(arg1, arg2)
			return anonym_func
		return curry_func

Such as, "curry_2_arg_func(pow)(2)(3)" will return what "pow(2)(3)" returns.

Generally, *taking abstraction after several instances being shown up and taking the general notaion, indeed, is the general way of abstraction, not only at here, but also in (almost) any other area of human cognition!* For this, see also the instance in [this](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#functions-as-arguments) section in the website.

### Lambda Expression

	     lambda            x            :          f(g(x))
	"A function that    takes x    and returns     f(g(x))"

Such as the previous instance of composition,

	def compose1(f, g):
		return lambda x: f(g(x))

which originally is (re-written)

	def compose1(f, g):
		def h(x):
			return f(g(x))
		return h


Some programmers find that using unnamed functions from lambda expressions to be shorter and more direct. However, compound lambda expressions are notoriously illegible, despite their brevity. The following definition is correct, but many programmers have trouble understanding it quickly.

	>>> compose1 = lambda f,g: lambda x: f(g(x))

(Remind that _Programmes are written for human-being to read. By a happy accident, however, it can run on a computer._)

In general, Python style prefers explicit def statements to lambda expressions, but allows them in cases where a simple function is needed as an argument or return value.

#### Etymology

The term _lambda_ is a historical accident resulting from the incompatibility of written mathematical notation and the constraints of early type-setting systems.

> It may seem perverse to use lambda to introduce a procedure/function. The notation goes back to Alonzo Church, who in the 1930's started with a "hat" symbol; he wrote the square function as "ŷ . y × y". But frustrated typographers moved the hat to the left of the parameter and changed it to a capital lambda: "Λy . y × y"; from there the capital lambda was changed to lowercase, and now we see "λy . y × y" in math books and (lambda (y) (\* y y)) in Lisp.

—Peter Norvig (norvig.com/lispy2.html)

Despite their unusual etymology, lambda expressions and the corresponding formal language for function application, the _lambda calculus_, are fundamental computer science concepts shared far beyond the Python programming community. We will revisit this topic when we study the design of interpreters in Chapter 3.


### First-class Status

In general, programming languages impose restrictions on the ways in which computational elements can be manipulated. Elements with the fewest restrictions are said to have _first-class status_. Some of the "rights and privileges" of first-class elements are:

1. They may be bound to names.
1. They may be passed as arguments to functions.
1. They may be returned as the results of functions.
1. They may be included in data structures.

Python awards functions full first-class status, and the resulting gain in expressive power is enormous.

## Recursive Function

### [Instance: Pig Latin](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#recursive-functions)

	def pig_latin(w):
        """Return the Pig Latin equivalent of a lowercase English word w."""
        if starts_with_a_vowel(w):
            return w + 'ay'
        return pig_latin(w[1:] + w[0])
	def starts_with_a_vowel(w):
		"""Return whether w begins with a vowel."""
		c = w[0]
        return c == 'a' or c == 'e' or c == 'i' or c == 'o' or c == 'u'

	pig_latin('pun')

By its environment diagram shown in the web-page, we find that, calling pig\_latin('pun') will create two frames of pig\_latin, even though this recursive function is intrinsically iterative! This is reasonable, since they have different local variables: for the first frame, w is bound to 'pun'; while for the second, w is bound to 'unp'.

(Recursive function that is intrinsically recursive is something like

	def factorial(n):
		if n == 1:
			return n
		return n * factorial(n-1)

Calling factorial(3) will return

	3 * factorial( 2 * factorial(1) )

which naturally involves three different local frames of factorial().)

### Different Patterns of Recursion

#### [Mutual Recursion](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#id45)

#### [Tree Recursion](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#id46)

In the instance therein, fib(3), for instance, is calculated many times. This repetation is a waste of resource!!

One should not conclude from this difference that tree-recursive processes are useless. When we consider processes that operate on hierarchically structured data rather than numbers, we will find that tree recursion is a natural and powerful tool. Furthermore, tree-recursive processes can often be made more efficient, as we will see in Chapter 3.




## Summary of Chapter 1

### Conception Review


* Environment

	The state of your computer (including its RAM & CPU (what are your computer doing temporarily?)).

    * Frame (Global & Local)

	    Each call of a function will evoke a local frame (at least for binding argument-name with value bounded to it)!

		* Environment diagram: *Extremely useful for the understanding of programme.*

	* Local variable

		In Python, any variable included in def-syntax will always be treated as local, _including arguments themselves_.

* Evaluate V.S. Execute

	Definition:

	* Evaluate is a map from the set combined by the set of expressions and the set of enviroments to (their indicated) values.

	* Execute a map from the set of environments to itself.


* High-order function

	Functions that maps on or//and onto the set of functions.

	* Closure
	
		Definition: def-syntax involves def-syntax.

		Function: If a function Ff is specially defined for defining function F, out of which it is useless, then define F involving the definition of Ff.

	* Currying

		Definition: func(arg1)(arg2)...(argn)

		Function: ???

	* Anonymous function: via lambda expression

		Function: Obvious. You need not to struggling with naming functions if their names themselves are not essential.

	* First-class status
		Python awards functions full first-class status.
		Function: For better abstraction.

* Recursive function

	Patterns:

		* Mutual recursion

		* Tree recursion


### Basic Python Syntax

* def

		def <name>(<formal parameters>):
			return <return expression>

* docstring & Testing via docstring


		def <name>(<formal parameters>):
			"""
				<docstring>
			
				>>> <test>
				<result of test>
			"""
			<codes>

* Comment (out): \#

* if-else

		if <expression true?>:
			<suite>
		elif <expression true?>:
			<suite>
		else:
			<suite>

* while

		while <expression true?>:
			<suite>

* from import

		from <module> import <module>

for instance,
			
		from operator import mul


### Habit for a Better Coding

* Function signature
	Remembering the function not only by its function-name, but also by its arguments-name, like the gismu in Lojban.
* Using pure function
	No side-effect, so that composing and testing can be done without extra caring, since a pure function always returns the same thing for the same inputs.
* Naming functions and parameters
	Several rules.



### Abstraction

The important thing is that *do not to write the same thing twice!* These words directly lead to _abstraction (of pattern)_ in programming. To illustrate this, a concise (too concise to be re-stated) instance is shown in the [lecture](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html#functions-as-arguments)!!

First-class status setting of Python provides vast tools for establishing this abstraction (of pattern)!


### These are all what I learned in chapter one.


EOF
