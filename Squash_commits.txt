Once you have rebased your work on top of the latest state of the upstream master, you may have several commits related to the issue you were working on. Once everything is done, squash them into a single commit with a descriptive message, like "Issue #100: Retweet bugfix."

To squash four commits into one, do the following:

$ git rebase -i HEAD~4

In the text editor that comes up, replace the words "pick" with "squash" next to the commits you want to squash into the commit before it. Save and close the editor, and git will combine the "squash"'ed commits with the one before it. Git will then give you the opportunity to change your commit message to something like, "Issue #100: Fixed retweet bug."

Important: If you've already pushed commits to GitHub, and then squash them locally, you will have to force the push to your branch.

$ git push origin branch-name --force

Helpful hint: You can always edit your last commit message, before pushing, by using:

$ git commit --amend

Source: https://github.com/todotxt/todo.txt-android/wiki/Squash-All-Commits-Related-to-a-Single-Issue-into-a-Single-Commit
