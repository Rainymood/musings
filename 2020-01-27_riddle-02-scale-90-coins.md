# Puzzle #2: 90 coins and an old-fashioned balance

WIP

From [here](http://puzzles.nigelcoldwell.co.uk/)

## Problem

>You are given a set of scales and 90 coins. The scales are of the same type
as above. You must pay $100 every time you use the scales.

>The 90 coins appear to be identical. In fact, 89 of them are identical, and
one is of a different weight. Your task is to identify the unusual coin and
to discard it while minimizing the maximum possible cost of weighing (another
task might be to minimizing the expected cost of weighing). What is your
algorithm to complete this task? What is the most it can cost to identify the
unusual coin?

## Solution

I know from the 12 balls problem that with a scale the most information you can gather from 1 weighing is on 3 groups. 

My intuition is to divide 90 balls into group A, B, C all 30 balls each and weigh them. 

Weight A against B. If it is unbalanced we can discard group C and continue with 60 balls. If it is balanced we discard A and B and continue with group C which is 30 balls.  

My god I remember this genius stackoverflow post from someone ... but I can't recall it. 

## Concepts

## Other problems

* [Problem #3](2020-01-27_riddle-03-bug.md)
* [Problem #5](2020-01-27_riddle-05-clock.md)
* [Problem #52](2020-01-27_riddle-52-socks.md)
* [Problem #68](2020-01-27_riddle-68-red-blue.md)