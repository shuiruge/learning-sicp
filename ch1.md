This draft based on the lecture at [here](http://inst.eecs.berkeley.edu/~cs61a/book/chapters/functions.html).

## Environment

> A name evaluates to the value associated with that name in the current environment.

This quotation recovers the reason of naming it as "environment". Indeed, _embeding_ a name (not string) into an environment is to look up the temporal memory to find out what this name is binded by. A name has its meaning only when the environment embeding it is indicated.


## Evaluate and Execute


1. "Evaluate" means a map from the set combined by the set of expressions and the set of enviroments to (their indicated) values.
1. "Execute" means a map from the set of environments to itself.

> Each type of expression or statement has its own evaluation or execution procedure.
