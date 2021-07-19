## `git update-index --skip-worktree`

This command will tell `git` to add a `skip-worktree` flag to the object on a path. When `git` reads this flag it will pretend its working directory version is up to date and it will read the index version instead.

### Why?

This is very useful in situation where you need to make local changes to some file (usually configuration) that can't be committed or ignored using `.gitignore` (because it needs to be part of the repository).

Often issue this command is designed to solve is when you end up having config file(s) with local specific settings that are always unstaged. They show up in `git status` and because of that tend to be wrongly committed as a part of a PR.

### How?

Running:

```sh
git update-index --skip-worktree path/to/config/file
```

will ignore local changes to specified file.

### Undo

To remove this flag run

```sh
git update-index --no-skip-worktree path/to/config/file
```

### How to find all files that have this flag set?

As pointed out in this [stackoverflow answer](https://stackoverflow.com/a/42363882/521373):

```sh
git ls-files -v . | grep "^S"
```

**Explanation:** `git ls-files .` lists all files in the repo. `-v` makes the output verbose, meaning that it will abbreviate the file status with a letter in front of the filename. The options are:

    H cached

    S skip-worktree

    M unmerged

    R removed/deleted

    C modified/changed

    K to be killed

    ? other

[Documentation](https://git-scm.com/docs/git-ls-files#Documentation/git-ls-files.txt-H)
