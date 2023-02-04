## Content

### Comparing `git` commands `push`, `fetch`, and `pull`

The `git` commands `push`, `fetch`, and `pull` have complementary actions for changes to a branch and repository (repo).  Each command manages changes between a local repo and a remote repo and their respective branches. The list below describes how each command works.

- [`git push`](#using-git-push) - sends changes from a local branch to a remote repo
- [`git fetch`](#using-git-fetch) - retrieves changes from a remote repo into your tracking branch
- [`git pull`](#using-git-pull) - retrieves changes from a remote branch into your tracking branch then merges them into a local branch

#### Using `git push`

This command sends changes from the current branch of the local repo to a remote repo. It first checks for a connected tracking branch on the remote repo. When confirmed, the changes then move to the remote branch for the repo.

After using `git push`, the remote branch then has the same changes as the local branch for the repository.

See the [Common issues](#common-issues) section for more about possible problems with using the command.

#### Using `git fetch`

The `fetch` command retrieves changes from a remote branch in a repo. It first confirms a connected tracking branch before retrieving the changes. After, it brings the changes to the tracking branch. This command doesn't change the local branch, however.

To bring the changes into your local repository, use `git merge origin/master` for the `master` branch. Use this combination of commands for more transparency in understanding the changes between the local and remote branches before merging.

#### Using `git pull`

The `git pull` command combines the `fetch` and `merge` actions into one. It retrieves the changes, confirms the connected tracking branch, then merges the changes into the local branch. This command is a common practice as it reduces the number of commands to run.

### Common issues

The section below covers how commands may not work as intended.

#### Diverging branches

The `git push` command can fail if the remote branch diverges from the local branch. In this situation, not all the commits in the remote branch are in the local branch. You can resolve this issue with two solutions.

- Synchronize your remote branch with `git pull` 
- Use `git fetch`, then `git merge` to synchronize the branches
