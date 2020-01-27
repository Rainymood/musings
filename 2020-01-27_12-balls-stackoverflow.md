# Twelve balls and a scale

I just want to take some time out of your day to highlight this glorious solution to the 12 balls and a scale problem. Please note that this is not my work, I just want to showcase its genius. 

From [here](https://puzzling.stackexchange.com/questions/183/twelve-balls-and-a-scale)

## The problem

>You are given twelve identical-looking balls and a two-sided scale. One of the balls is of a different weight, although you don't know whether it's lighter or heavier. How can you use just three weighings of the scale to determine not only what the different ball is, but also whether it's lighter or heavier?

## [Joe Z's solution](https://puzzling.stackexchange.com/a/224)

The key to these kind of problems is: **How much information can you get from the procedures you are allowed to undertake.**

With each weighing, the scale can tip left, right, or stay balanced.

In other words there are 3 outcomes per weighing. 

With 3 weighings there are 3^3=27 distinct outcomes. 

In this problem we have 24 results: ball `i` is `j` where `i=1,...,12` and `j=heavier,lighter`. So `2x12=24` results. 

Now we are going to map out each result to an outcome. 

Furthermore, each state of the ball can be either: on the left, on the right, or off the scale. 

If the odd ball out is heavier then: 

* and the ball is placed on the left side, the scale will tip to the left.

* and the ball is placed on the right side, the scale will tip to the right.

* and the ball is off the scale, the scale will remain balanced.

If the ball is lighter, then the first 2 cases are inverted.

There are 27 possible ways to place each ball in 3 weighings, `[left, right,
off]^[w1, w2, w3]`.

Each of these ways corresponds to a different outcome if the given ball is lighter or heavier. 

**This is key** We need to set-up an arrangement such that each possible set of placements and its inverse **is distinct**. 

```
Ball  1  2  3  4  5  6  7  8  9  10 11 12  -1 -2 -3 -4 -5 -6 -7 -8 -9 -10-11-12
W1    L        L  L     L  L     L  L  R    R        R  R     R  R     R  R  L
W2       L     R     L  L     L  L  R  L       R     L     R  R     R  R  L  R
W3          L     R  R     L  L  R  L  L          R     L  L     R  R  L  R  R

L = place it on the left                  This second table lists the inverses.
R = place it on the right                 Notice that no possible arrangement
  = leave it off                          appears more than once in both tables.
```

This arrangement satisfies the distinctness, but won't work, because we are not putting the same amount of balls on each scale. 

In other words, we need to fiddle around a bit under the restriction that we are putting 4 balls on each side for weighing. A bit of trial and error gives you this. 

```
Ball  1  2  3  4  5  6  7  8  9  10 11 12  -1 -2 -3 -4 -5 -6 -7 -8 -9 -10-11-12
W1    L        L  R     R  L     R  R  L    R        R  L     L  R     L  L  R
W2       L     R     L  R     L  R  L  R       R     L     R  L     R  L  R  L
W3          R     L  R     L  L  L  R  R          L     R  L     R  R  R  L  L
```

In other words, we label the balls 1 to 12 and weigh the following balls against each other

```
Weighing 1:  1  4  8 12 /  5  7 10 11
Weighing 2:  2  6  9 11 /  4  7 10 12
Weighing 3:  5  8  9 10 /  3  6 11 12
```

We record the results of the experiments with `L, =, R` (tips left, stays
balanced, tips right) and use the following table to find out which ball
heavier/lighter.

```
= = L : 3L    L = = :  1H   R = = :  1L
= = R : 3H    L = L :  8H   R = L :  5H
= L = : 2H    L = R :  5L   R = R :  8L
= L L : 9H    L L = :  7L   R L = :  4L
= L R : 6H    L L R : 10L   R L L : 12L
= R = : 2L    L R = :  4H   R L R : 11H
= R L : 6L    L R L : 11L   R R = :  7H
= R R : 9L    L R R : 12H   R R L : 10H

= : scale balanced
L : scale tipped to the left
R : scale tipped to the right
nL : ball n is light
nH : ball n is heavy
```

## Concepts and comments

I was, personally, blown away by this solution. 

What concepts are being used? 

**Figure out the information gathering power of your actions**

How much information (configurations) can you figure out, given your allowed actions. Here we have 3 weighings with 3 results each weighing so we can have 27 unique end states. 