# Refactoring a codebase

Some simple guidelines to keep in the back of your mind when refactoring a
codebase.

## Guideline 1: Write short units of code

* Are there functions that are >15 LOC? 
    * Can we split out methods/units/functions? 

## Guideline 2: Write simple units of code 

* Are there functions with >4 branching points??
    * Can we split out branching points into data structures? 
    * Can we replace conditionals with polymorphisms? 
    * Can we replace nested conditionals with guard clauses? 

## Guideline 3: 

* Are there functions with >4 parameters?
    * Can we group these together in their own objects/functions?

## Other

* Is there code that is not used at all? 
    * Can we delete it? Do the tests still pass or do we need to update them?
* Is the documentation still up to date? 
    * Do we have to rewrite the docstrings/documentation? 
* Are there TODOs/fix later comments in the code? 
    * Are they still relevant? Can we refactor? 
* Are there magic numbers in functions? 
    * Can we make them visible? Refactor them into `MAGIC_NUMBER` global variables? 