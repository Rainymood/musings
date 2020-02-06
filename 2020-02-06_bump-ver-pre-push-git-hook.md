# Saving more than $20.000\* with git hooks

## The problem

Although we have a sophisticated CI/CD pipeline, our complete local to production pipeline still takes
roughly 1 hour in its totality. 

It has happened before that we forgot to update the version in our
`package.json` when deploying to production. When this happens our pipeline
fails, then we have to bump the version and redeploy, wasting an hour. This is
the problem I want to solve.

## The solution

The solution I have in mind is a pre-push git hook that checks whether there
is a difference between the version in `package.json` in the local branch and the remote branch we want to push to (develop, in our case). The pre-push commit hook should return an error if it does not find a difference in the versions, and hence we forgot to update.

Thankfully, someone did most of this work for us already
[here](https://stackoverflow.com/questions/58049582/how-to-check-package-lock-json-for-semantic-version-bump-using-pre-push-git-hook).

```bash
while read local_ref local_sha remote_ref remote_sha
do
        if git diff $remote_sha..$local_sha package.json | grep -q version;
        then
                echo "Bumped version. Pushing..."
                exit 0;
        else
                echo "Did not bump version"
                exit 1;
        fi
done
```

Create a new file with `touch .git/hooks/pre-push` and paste the script in
there.

Make sure make the hook executable with `chmod +x .git/hooks/pre-push`.

```bash
$ touch tmp.txt
$ git add tmp.txt
$ git commit -m "adds tmp.txt" 
$ git push
Did not bump version
error: failed to push some refs to 'https://github.com/Rainymood/...'
```

Nice! If we try to push without updating the `package.json` version our
pre-push git hook stops us!

```bash
$ vim package.json # bump version
$ git add package.json
$ git commit -m "bump ver"
$ git push
Bumped version. Pushing...
...
To https://github.com/Rainymood/...
   58ff36e..7c7546c  master -> master
```

After bumping the version we can push.

## Why this works

Honestly, I was not aware of this but as it turns out each `.git` folder has a `.git/hooks` folder with examples in there!

```bash
$ ls .git/hooks
.git/hooks:
applypatch-msg.sample     fsmonitor-watchman.sample pre-applypatch.sample     pre-push.sample           pre-receive.sample        update.sample
commit-msg.sample         post-update.sample        pre-commit.sample         pre-rebase.sample         prepare-commit-msg.sample
```

For example

```bash
$ cat .git/hooks/pre-push.sample
#!/bin/sh

# An example hook script to verify what is about to be pushed.  Called by "git
# push" after it has checked the remote status, but before anything has been
# pushed.  If this script exits with a non-zero status nothing will be pushed.
#
...
```

First, how do we get `remote_sha` locally? 

```
$ git ls-remote https://github.com/Rainymood/... | head -1
<remote_sha>
```

Second, how do we get `local_sha` locally? See [this](https://stackoverflow.com/questions/20873649/show-sha1-only-with-git-log) post.

```bash
$ git log --format=format:%H | head -1
<local_sha>
```


After updating the version in `package.json` and diffing we get this

```bash
$ git diff <remote_sha>..<local_sha>
 {
   "name": "svelte-app",
-  "version": "1.0.0",
+  "version": "1.0.1",
...
```

Trying out the full command

```bash
$ git diff <remote_sha>..<local_sha> | grep -q version
```

Returns nothing, but exits with a success (exit code `0`). This works because
of the `-q` (quiet) flag that we add. View the documentation
[here](https://linux.die.net/man/1/grep).

>Quiet; do not write anything to standard output. Exit immediately with zero
status if any match is found, even if an error was detected. Also see the -s
or --no-messages option. (-q is specified by POSIX .)

If we did not update the version in `package.json` it would exit with an
error (exit code `1`) because it did not find a match. 

## The impact

**We estimate the impact of this fix is more than $21.500**

Of course the total impact is hard to quantify, but a quick and dirty `git
log | grep -c ver` results in 215 matches. Let us now assume that we only
make the commit message "bump version" if we forget to do so then this has
happened already 215 times. Estimating developer time at $100/hr which is not
unreasonable we get a rough estimate of around $21.500 that we saved.

## Disclaimer

\*Of course, the number $20.000 is a coarse estimate but still a good
exercise to quantify your own impact. Honestly, I think that the number is even higher because we only looked at the past savings. We should extrapolate the number to all the future version bumps that we would have missed. Oh well, such is life. 

## Links

* Atlassian tutorial on git hooks [here](https://www.atlassian.com/git/tutorials/git-hooks)
* Man grep for `-q` can be found [here](https://linux.die.net/man/1/grep)
