# Writing maintainable software Chapter 10 (testing)

## Five types of tests 


| Type             | What it tests                                                                     | Why                                           | Who                                           |
|------------------|-----------------------------------------------------------------------------------|-----------------------------------------------|-----------------------------------------------|
| Unit test       | Functionality of one unit in isolation                                            | Verify that the unit behaves as expected      | Developer (preferably of the unit)            |
| Integration test | Functionality, performance, or other quality characteristic of at least 2 classes | Verify that parts of the system work together | Developer                                     |
| End-to-end test  | System interaction (with a unit or other system)                                  | Verify that the system behaves as expected    | Developer                                     |
| Regression test  | Previously erroneous behavior of a unit, class, or system interaction             | Ensure that bugs do not reappear              | Developer                                     |
| Acceptance test  | System interaction (with a user or another system)                                | Confirm the system behaves as required        | End-user representative (never the developer) |

It is crucial for all developers to learn how to write unit tests well. 

## Principles for good unit tests

1. Test both normal and special cases
    * Test the happy flow. Write tests that cover that the unit behaves like you want it to.
    * Test the sad/rainy day flow. Write tests that confirm a unit behaves sensibly on non-normal or faulty input. 
2. Maintain tests just like nontest (production) code
    * If you change a unit, also change the tests, and vice versa. 
3. Write isolated tests
    * Never write tests depending on the state or other files written by other tests. If you need data then *stub* or *mock* it. 

## What is test-driven development?

The approach of writing a unit test before writing the code that conforms to
the test. Then writing the minimum amount of code to make the test pass. 

Designing a method becomes easier if you think about how you are going to
test it. What are the valid inputs/parameters, and what should the method
return as a result?

## Summary

* Automate tests
* Use test-driven development (TDD)