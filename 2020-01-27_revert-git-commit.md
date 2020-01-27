# How to revert a git commit

Today I was working on the master branch and accidentally pushed code without
realising it. Yikes!

Or in other words: Oh shit, Git!

## Summary

So how do you "revert" a commit? In a nutshell: 

* `git log`
* Grab last commit hash, say `<commit_hash>`
* `git revert <commit_hash>`

Technically speaking, this makes another commit which revers the last commit.
So it doesn't actually *delete* the old commit, it just reverts it by making
another one.

## What happened

This is what I did 

```bash
api on master [$+] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git add .

api on master [$+] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git commit -m "Adds sed command to replace internal chatbase URL to outbound gateway URL" 
...
 1 file changed, 4 insertions(+)

api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git push 
...
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 4 threads
Compressing objects: 100% (3/3), done.
```

Oh no! I pushed to the master branch. 

## Watch me mess up twice

`git log` to the rescue (Narrator: Or so he thought.)

```bash
commit 487820df353f44e1348597a2fa90b9c1b78ebecb (HEAD -> master, origin/master, origin/HEAD)
Author: Jan Meppe
Date:   Mon Jan 27 16:01:31 2020 +0100

    Adds sed command to replace internal chatbase URL to outbound gateway URL

commit 3b771091ecc657b1ab3319510beb99fe9709494f
Merge: ca1fc58 16624bd
Author: Jan Meppe
Date:   Fri Jan 24 11:15:19 2020 +0100

    Merge branch 'master' of https://...
```

Trying this we get

```bash
api on master [$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git revert 3b771091ecc657b1ab3319510beb99fe9709494f
error: commit 3b771091ecc657b1ab3319510beb99fe9709494f is a merge but no -m option was given.
fatal: revert failed
```

Because the last commit is a merge. 

What we are looking for 

```bash
git revert -m 1 <merge commit hash>
```

However, a word of caution 

>Note that you cannot re-merge the branch after this, as the docs says: "Reverting a merge commit declares that you will never want the tree changes brought in by the merge. As a result, later merges will only bring in tree changes introduced by commits that are not ancestors of the previously reverted merge. This may or may not be what you want." – Dalibor Karlović Nov 12 '15 at 9:44

From [this](https://stackoverflow.com/questions/11722533/rollback-a-git-merge) link. 

In our case, this is the correct approach. (Narrator: It wasn't.) 

```
api on master [$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git revert -m 1 3b771091ecc657b1ab3319510beb99fe9709494f
[master 60e4ae5] Revert "Merge branch 'master' of https://...
 8 files changed, 287 insertions(+), 281 deletions(-)
 rewrite src/config/index.js (96%)

api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 took 16s 
➜ git add .

api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 
➜ git log  
```

(Narrator: Here our subject realises he messed up, twice.)

NO wait I messed it up. 

I don't want to revert that one. I want to revert the last one. 

Now we need to undo the last commit and the one we reverted! 

OK we `git logged` and reverted the last 2 commits now. 

```bash
api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 took 8s
➜ git log
...
api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 took 8s
➜ git revert 60e4ae5ce266b8c60835aa0899750748000ca938
[master 167d3a0] Revert "Revert "Merge branch 'master' of https://...""
 8 files changed, 281 insertions(+), 287 deletions(-)
 rewrite src/config/index.js (96%)

api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 took 8s
➜ git log

api on master [⇡$] is 📦 v3.5.30 via ⬢ v12.13.0 on 🐳 v19.03.5 took 9s
➜ git revert 487820df353f44e1348597a2fa90b9c1b78ebecb
[master b54b3fb] Revert "Adds sed command to replace internal chatbase URL to outbound gateway URL"
 1 file changed, 4 deletions(-)
```