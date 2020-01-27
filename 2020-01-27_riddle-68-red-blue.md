# Riddle #68: Arrangements of Red & Blue Marbles

From [here](http://puzzles.nigelcoldwell.co.uk/)

## Problem

>You have 50 red marbles and 50 blue marbles, other wise identical. You have 2 identical jars. A marble will be selected at random, from a jar selected at random. How would you apportion the marbles so as to maximise the chances of a red marble being selected?
Can you prove it?

## Clue

>We're aiming quite high here the probability is going to be just under 75%. The graph here may be considered a clue.

>It's worth noting that the question does not say that there has to be 50 marbles in each jar.

>If we could get one of the jars up to 100% red without dropping the percentage red in the other jar too far...

>Where next?

## Solution

Two jars, lets call them jar 1 and jar 2

In total we have 50 red and 50 blue marbles

Denote R1 and B1 the amount of red and blue marbles in the first jar, respectively.

Denote R2 and B2 the amount of red and blue marbles in the second jar, respectively.

Of course, R2 = 50-R1 and B2=50-B1. 

We need to maximize P(R) 

P(R) = 1/2 * R1 / (R1 + B1) + 1/2 * R2 / (R2 + B2)

We can write out this formula and fill in values for R2 and B2 and try to solve this. 

But what is the intuition? 

What if we put a single red marble in the first jar and the other 49 in the second one. Then we get

P(R) = 1/2 * 1 + 1/2 ( 49 / 99) = 0.7474...

In other words, we run into an edge case. 

## Concepts

What problem solving concepts are used here? 

**Translating text to formulas** 

Translating text to formulas is definitely something that is used here. 

**Minimizing and maximizing**

Not directly applied, but I did try to simplify the problem to n=1 and n=inf

**Wrong hidden assumptions**

This is the key to this question. I struggled with this question because I incorrectly assumed that the amount of marbles should be equal in the jars. This is mentioned no-where! 

In other words, this "out of the box" answer only comes from your own wrong assumptions! 

## Other problems

* [Problem #3](2020-01-27_riddle-03-bug.md)
* [Problem #5](2020-01-27_riddle-05-clock.md)
* [Problem #52](2020-01-27_riddle-52-socks.md)
* [Problem #68](2020-01-27_riddle-68-red-blue.md)