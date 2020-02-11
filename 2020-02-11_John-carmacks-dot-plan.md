# John Carmack's dot plan

From [here](https://garbagecollected.org/2017/10/24/the-carmack-plan/)

>As life in the war room pressed on, Carmack took it upon himself to let gamers know that, yes, id really was moving along with its work on Quake. So he decided to upload his daily work log, or, as it was known, a .plan file, to the Internet. Plan files were often used by programmers to keep each other informed of their efforts but had yet to be exploited as means of communicating with the masses. But id’s fans had suffered months, years, of Romero’s unsubstantiated hyperbole, Carmack felt and it was time that they saw some hard data.

This daily work log used the [Name/Finger](https://en.wikipedia.org/wiki/Finger_protocol#Finger_user_information_protocol) protocol. If you would finger John Carmack, this is what you would see:

```
[idsoftware.com]
Login name: johnc In real life: John Carmack
Directory: /raid/nardo/johnc Shell: /bin/csh
Never logged in.
Plan:

This is my daily work ...

When I accomplish something, I write a * line that day.

Whenever a bug / missing feature is mentioned during the day and
I don’t fix it, I make a note of it. Some things get noted many times
before they get fixed.

Occasionally I go back through the old notes and mark with a +
the things I have since fixed.

--- John Carmack

= feb 18 ===================================
* page flip crap
* stretch console
* faster swimming speed
* damage direction protocol
* armor color flash
* gib death
* grenade tweaking
* brightened alias models
* nail gun lag
* dedicated server quit at game end
+ scoreboard
+ optional full size
+ view centering key
+ vid mode 15 crap
+ change ammo box on sbar
+ allow “restart” after a program error
+ respawn blood trail?
+ -1 ammo value on rockets
+ light up characters
```

[Here](https://github.com/floodyberry/carmack/blob/fc09ed3e7dde67b296a6840524a7c9ec8c36511a/plan_files/johnc_plan_19960218.txt) is another example from 1996-02-19

```
*	fixed vanished bmodels at startup
*	fixed getting stuck problem
*	fixed the sound out of range problem
*	hack patched the dissapearing under water

+ remove look spring
+ fix radius damage
+ too many people on status bar
+ invis in water
+ multiple pain sounds started
+ shift jumps while in console
+ less blood time
+ gone armor icon
+ look up/down keyboard modifier

clamp velocity on respawn
clear damage flash on respawn
bad changeup 
bob speed kept on restart
no puffs in sky volumes
id copyright seal
teleport screen warp
```

Note that not all `.plan` contained lists of tasks, some of them are opinion
pieces or streams of thought. I find these super interesting to read.
Artifacts from a long time ago. 

The format [from the man himself](https://github.com/floodyberry/carmack/blob/fc09ed3e7dde67b296a6840524a7c9ec8c36511a/page_tools/template/plan.html#L26-L41)

>When I accomplish something, I write a * line that day. 

>Whenever a bug / missing feature is mentioned during the day and I don't 
fix it, I make a note of it.  Some things get noted many times before 
they get fixed.

>Occasionally I go back through the old notes and mark with a + the things
I have since fixed.
>Later entries may use:
>* An empty entry was a note
>* A `*` entry was completed on that day 
>* A `+` entry was completed on a later day
>* A `-` entry was decided against on a later day.

## My `.plan` for today

This is what my `.plan` would look like for today.

```
==========
2020-02-11
==========

* implement hot reload Docker
* calculated impact hot reload
* write hot reload Docker post
* write how to add volume but exclude subfolder post
* write mac ubuntu compiled docker does not work post
* write docker volume mount on runtime post
* write dot plan post
* review narendra PR
* write good commit message post
* write 2nd draft of how to get rich
* write yuval post on storytelling

fix vpn
send mail to manager project
```