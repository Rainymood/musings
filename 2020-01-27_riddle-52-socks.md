# Puzzle #52: Different Coloured Socks in a Drawer

From [here](http://puzzles.nigelcoldwell.co.uk/)

## Problem

>52. 100 black and white socks are in a drawer. How many socks must you pull out before you are guaranteed to have a pair? Generalize to socks of N different colors.

## Solution

The answer is 3 socks for N=2 colors. 

To generalise this to N colors we need to draw we need to draw N+1 socks. 

This uses the reverse [pigeonhole principle](https://en.wikipedia.org/wiki/Pigeonhole_principle).

Basically. If you have N different pigeonholes and N+1 pigeons, then at least 1 box will always have 2 or more pigeons. In the "best" case every pigeon fills one hole and one hole has two. In the worst case, they are all jammed into one pigeonhole. 

Same with socks, but then in reverse. 

## Concepts

What problem solving concepts are used here? 

**The pigeonhole principle**

From wikipedia: 

>In mathematics, the pigeonhole principle states that if $n$ items are put into $m$ containers, with $n>m$, then at least one container must contain more than one item.[1] In layman's terms, if you have more "objects" than you have "holes," at least one hole must have multiple objects in it.

## Other problems

* [Problem #3](2020-01-27_riddle-03-bug.md)
* [Problem #5](2020-01-27_riddle-05-clock.md)
* [Problem #52](2020-01-27_riddle-52-socks.md)
* [Problem #68](2020-01-27_riddle-68-red-blue.md)