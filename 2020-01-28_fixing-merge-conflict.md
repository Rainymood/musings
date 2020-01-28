# How to fix a merge conflict

## Summary

* `git push` Oh shit! Merge conflict upstream!
* `git checkout develop` Assuming you branch from develop
* `git pull`
* `git checkout feature/TICKET-500`
* `git pull origin develop`
* Fix merge conflict
* `git add, commit, push` 
* DONE! 

## Example

```bash
➜ git checkout develop
Switched to branch 'develop'
Your branch is behind 'origin/develop' by 224 commits, and can be fast-forwarded.
  (use "git pull" to update your local branch)

➜ git pull
Updating 92c41a5..e8fe917
Fast-forward
...
29 files changed, 5943 insertions(+), 962 deletions(-)
delete mode 100644 scratch.js

➜ git checkout feature/TARA-943-v2
Switched to branch 'feature/TARA-943-v2'
Your branch is up to date with 'origin/feature/TARA-943-v2'.

➜ git pull origin develop
From https://...
 * branch            develop    -> FETCH_HEAD
Auto-merging src/config/index.js
CONFLICT (content): Merge conflict in src/config/index.js
Automatic merge failed; fix conflicts and then commit the result.

➜ code .
...

➜ git add .
...

➜ git commit -m "Fixes merge conflict"
[feature/TARA-943-v2 ae910de] Fixes merge conflict

➜ git push
...
```