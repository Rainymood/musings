# The `&&` and `||` operators in bash

`&&` and `||` are two logical operators in bash. This is how they work.

## `&&` or the logical-`and` 

The `&&` operator works like a logical-`and`. Because `and` requires both the
left and the right hand operator to be true it evaluates the right hand side
if the left hand side is also true.

**In other words: the right side of `&&` gets executed only if the exit
status of the left side is 0 (true)**.

## `||` or the logical-`or` 

The `||` operator works like a logical-`or`. Because `or` requires only one
of the sides to be true, if it finds a true value it exists and only if the
first value is false (non-zero) it evaluates the right hand side.

**In other words: the right hand side of `||` gets executed only if the exit
code of the left side is non-zero (false, non-zero exit code).**
