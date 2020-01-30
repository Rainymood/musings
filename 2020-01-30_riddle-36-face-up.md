# Puzzle #36: N Face Up Cards In A Dark Room

From [here](http://puzzles.nigelcoldwell.co.uk/)

## Problem

>You are in a dark room with a deck of cards. N of the cards are face up and
the rest are face down. You can't see the cards. How do you divide the deck
in to two piles with equal numbers of face up cards in each?

## Solution

The solution to this question is mind-blowing and really simple and elegant. There is no slight of hand here. 

In a deck with `N` cards facing up in a deck of `K` cards. Simply take any combination of `N` cards, split them off in a separate pile, and then flip them. 

## Concepts

**Hidden assumptions** 

Again this question is rife with hidden assumptions. 

For example: 

* The question does **not** say that we need to create two equal piles of 26
cards. It just says two piles with the same amount of face up cards.

* The question does **not** say that we need the same cards to be facing up
that are face up at the start. In other words, we can flip the cards. This
increases the action space.

* The question **does** say we know the number of face up cards. 

We have to be careful to read the question carefully and not make any assumptions that are wrong. Perhaps it makes sense to state explicitely the assumptions that we make and check them.

**Information gathering**

Again, the solution to this problem as to do with the amount of information
that you can gather per action that you can take. Which is a recurring
pattern I've seen in multiple questions now.

Consider briefly what actions you can do. 

You are in the dark room, right? So what actions are there available to you? 

* Denote `K` the amount of cards in the deck

* Denote `N` the amount of cards that are face up

The only sensible actions that you can take are: 

* Split off `1...K` cards into a separate pile

* Flip `1...K` cards

Then simply you could brute force the solution if you look calmly at the
sensible actions that you can take.

## Other problems

* [Problem #3](2020-01-27_riddle-03-bug.md)
* [Problem #5](2020-01-27_riddle-05-clock.md)
* [Problem #7](2020-01-30_riddle-07-lights.md)
* [Problem #8](2020-01-30_riddle-08-prime.md)
* [Problem #36](2020-01-30_riddle-36-face-up.md)
* [Problem #52](2020-01-27_riddle-52-socks.md)
* [Problem #68](2020-01-27_riddle-68-red-blue.md)