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

When in doubt, write down the recurrence relationship.

Whenever possible, apply memoization.

When stack overflows, tail recursion might help.

Quick sort first selects a value from the list which serves as a pivot value to divide the list into two sublists. One sublist contains all the values that are less than the pivot value, while the other sublist contains the values that are greater than or equal to the pivot value, this is called partitioning. the strategy of choosing a pivot value can vary. 

after partioning, the original list is then reduced into two smaller sublists. we then recursively sort the two sublists. 

we are sure that all elements in one sublists are less or equal than any element in another sublist. Therefore, we can simply concatenate the two sorted sublists that we obtain before to obtain the final sorted list.

Master Theorem, also known as Master Method, provides asymptotic analysis (i.e. the time complexity) for many of the recursion algorithms that follow the pattern of divide-and-conquer.


