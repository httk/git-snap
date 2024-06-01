# Git-snap

Plugin for git to aid with making snapshots of work-in-progress states of git repos.

## Setup and use

You just need a git remote to push the snapshots to, preferably named 'snaps':
```
git remote add snaps <remote server specification>
```

Now, if you are working on a branch named `BRANCH`, you can make a snapshot and push it to the remote by:
```
git snap
```

This will create a new snapshot branch if it does not already exist named `snapshots/BRANCH/<hostname>`.
On this branch it will merge in any commits from BRANCH that are not already on this branch, and then add a new commit for the file changes in tracked and untracked files made since that commit.

If you work on multiple computers, push the snapshots to the same remote.
The inclusion of `<hostname>` in the branch name keeps them separate.

You should generally avoid doing `checkout` on the snapshot branches (the snapshot system uses worktrees, and
it is not possible to have the same branch in multiple worktrees.)
To browse snapshots, check instead out the corresponding commit (i.e., enter a detached HEAD state).

## Recovery from snapshots

Apart from not doing `checkout` on snapshot branches, they work as usual. I.e., you can use, e.g., `git diff snapshots/BRANCH/othercomp` to see how they differ, and merge changes from them if desired.

## Advanced use

You can see some other features by:
``
git snap help
``

But, in short, you can also do:

- `git snap make <snapshot branch>`

    Creates a snapshot on the specified `<snapshot branch>`, which doesn't need to follow the naming conventions used by just calling `git snap`.

- `git snap push <snapshot branch> <remote>`

    Pushes the contents of `<snapshot branch>` to the `<remote>`, handling `--set-upstream` and a few other minor details that makes sense specifically for snapshots.
