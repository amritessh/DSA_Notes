Recursion is an approach to solving problems using a function that calls itself a subroutine.

Each time a function calls itself, it reduces the given problem into sub  problems. 

The recursion call continues until it reaches a point where the sub problem can be solved without further recursion.

A recursive function should have - A simple base case: a terminating scenario that does not use recursion to produce an answer.
A set of rules also known as recurrence relation that reduces all other cases towards the base case.

We define the problem as the function F(X) to implement, where X is the input of the function which also defines the scope of the problem:
Then, in the function F(X), we will:
1. Break the problem down into smaller scopes.
2. Call Function recursively to solve the subproblems.
3. Finally, process the results from the recursive function calls to solve the problem corresponding to X.


