## How to revert a PR merge from console?

### Why?
If we try to revert a PR from GitHub website, and that PR has a merge conflicts, GitHub will simly show a message: "Sorry, this pull request couldn’t be reverted automatically". In that case we have to revert PR manually using CLI.

### How?
First find a merge commit on the branch where this PR is merged to.

> Note: This merge commit can't be seen in the PR itself, since it is one commit after the last commit in the PR.

Copy hash of that commit. Now, in console:

```sh
# checkout to the branch where PR was pointed to
git checkout branch-where-pr-landed

# make sure everything is up to date
git fetch
git pull origin branch-where-pr-landed

# create a new branch where code for the revert will be placed
git checkout -b rever-pr

# start revert
git revert -m 1 hash-of-the-merge-commit
```

The `-m 1` option tells Git that we want to keep the parent side of the merge (which is the branch we had merged into). ([Ref](https://www.git-tower.com/learn/git/faq/undo-git-merge/))

Now, resolve all conflicts. Use `git add` to mark files as resolved. After all is done compleate revert with:

```sh
git revert --continue
```

It is now ready to push this branch and create a PR from it.

### Reference

 - https://www.git-tower.com/learn/git/faq/undo-git-merge/
 - https://git-scm.com/docs/git-revert
