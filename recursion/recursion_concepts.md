Recursion is an approach to solving problems using a function that calls itself a subroutine.

Each time a function calls itself, it reduces the given problem into sub  problems. 

The recursion call continues until it reaches a point where the sub problem can be solved without further recursion.

A recursive function should have - A simple base case: a terminating scenario that does not use recursion to produce an answer.
A set of rules also known as recurrence relation that reduces all other cases towards the base case.

We define the problem as the function F(X) to implement, where X is the input of the function which also defines the scope of the problem:
Then, in the function F(X), we will:
1. Break the problem down into smaller scopes.
2. Call Function recursively to solve the sub problems.
3. Finally, process the results from the recursive function calls to solve the problem corresponding to X.

given a recursion algorithm its time complexity O(T) is typically the product of the number of recursion invocations and the time complexity of calculation that incurs along with each recursion call: O(T) = R*O(s)

recursion related space refers to the memory cost that is incurred directly by the recursion ie the stack to keep track of recursive function calls. in order to complete a typical function call, the system allocates some space in the stack to hold three important pieces of information:
1. the returning address of the function call, once the function call is completed, the program must know where to return to ie the line of code after the function call.
2. the parameters that are passed to the function call.
3. the local variables within the function call.




