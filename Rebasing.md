# Add the remote, call it "upstream":

git remote add upstream https://github.com/whoever/whatever.git

# Fetch all the branches of that remote into remote-tracking branches,
# such as upstream/master:

git fetch upstream

# Make sure that you're on your master branch:

git checkout master

# Rewrite your master branch so that any commits of yours that
# aren't already in upstream/master are replayed on top of that
# other branch:

git rebase upstream/master

git push -f origin master



---credits: stackoverflow ---
---Link: https://stackoverflow.com/questions/7244321/how-do-i-update-a-github-forked-repository ---

### rebasing interactively

If for some reason a change has to be made afterward, for whatever reasons: review, a bug fix, adding a missing stuff, javadoc etc. The history can then be chaotic.

```
* a2d6ee9 - Review fixes
* eed23a1 - Complete awesome feature with better error message
* 84aaad2 - wip
* 24fe90a - Merge from somewhere else
* 5a03bf5 - fix another bug
* ccc50cc - fix npe stuff
* 18d8ace - Added new awesome feature
```

Multiple commit just create noise in the PR, and they don't help to understand the change, even less when the history has to be read again.
To clean that history you have to use the git's _interactive rebase_

```bash
git rebase --interactive HEAD~7
```

It will open an editor with the `7` chosen commits (in the opposite order) :

```
pick 18d8ace Added new awesome feature
pick ccc50cc fix npe stuff
pick 5a03bf5 fix another bug
pick 24fe90a Merge from somewhere else
pick 84aaad2 wip
pick eed23a1 Complete awesome feature with better error message
pick a2d6ee9 Review fixes
```

For each line (commits) you can do different possible actions.

```
pick 18d8ace Added new awesome feature
fixup ccc50cc fix npe stuff
fixup 5a03bf5 fix another bug
pick 24fe90a Merge from somewhere else
squash 84aaad2 wip
squash eed23a1 Complete awesome feature with better error message
pick a2d6ee9 Review fixes
```

In the above abstract I wrote several action that git will perform interactively.
The `fixup` command tells git to amend the commit without changing the message.
The `squash` command tells git to amend the commit and ask in an editor what is the message of the new commit.
This will result in the following history :

```
* 72d6ac9 - Review fixes
* abe934f - Better error messages for awesome feature
* 73a6abe - Added new awesome feature
```

### just squashing

Sometime however having a single commit is just the right thing to do. With the following history :

```
* a2d6ee9 - Review fixes
* eed23a1 - Complete awesome feature with better error message
* 84aaad2 - wip
* 24fe90a - Merge from somewhere else
* 5a03bf5 - fix another bug
* ccc50cc - fix npe stuff
* 18d8ace - Added new awesome feature
```

You can do that 

```bash
git reset --soft HEAD~7
git commit --message="Adds awesome feature"
```


### pushing the branch with modified history

If everything is ok just perform a _push force_, it is required as the history has changed and git forbids push that modify the history on the remote, that's why we need to indicate git it's OK.

The following command specify the _remote_ and the branch, it is important if using git with a version inferior to 2.0.0, see more about the [push.default config](http://stackoverflow.com/questions/23918062/simple-vs-current-push-default-in-git-for-decentralized-workflow)

```bash
git push -f origin pr_that_improve_stuffs
```

Now you should see changes in the PR.

[Link](https://github.com/mockito/mockito/wiki/Using-git-to-prepare-your-PR-to-have-a-clean-history/)