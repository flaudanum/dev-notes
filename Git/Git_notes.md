# Git notes

- [Git notes](#git-notes)
  - [Configuration](#configuration)
  - [Clone repositories](#clone-repositories)
    - [Clone a branch that is different from the default one](#clone-a-branch-that-is-different-from-the-default-one)
  - [Export a local repository to GitLab or GitHub](#export-a-local-repository-to-gitlab-or-github)
  - [Remote repository management](#remote-repository-management)
    - [Set remote of a local repository](#set-remote-of-a-local-repository)
  - [Branch management](#branch-management)
    - [Create a new branch and change to that branch](#create-a-new-branch-and-change-to-that-branch)
    - [Create a new remote branch](#create-a-new-remote-branch)
    - [Change to a new local branch tracking a remote one](#change-to-a-new-local-branch-tracking-a-remote-one)
    - [Remote branch merging](#remote-branch-merging)
    - [Rebase a branch](#rebase-a-branch)
    - [Solve merge conflicts](#solve-merge-conflicts)
    - [Suppress a local branch](#suppress-a-local-branch)
    - [Suppress a distant branch](#suppress-a-distant-branch)
  - [Working tree and staging area](#working-tree-and-staging-area)
    - [Revert the state of modified files in the working tree](#revert-the-state-of-modified-files-in-the-working-tree)
    - [Bring back files from the staging area](#bring-back-files-from-the-staging-area)
    - [Discard every untracked modification](#discard-every-untracked-modification)
    - [Stashing untracked modifications](#stashing-untracked-modifications)
  - [Tagging](#tagging)
  - [Edit the last commit](#edit-the-last-commit)
  - [List files changed between two commits](#list-files-changed-between-two-commits)


## Configuration

List configuration:

```bash
git config --list
```

Set the global settings for properties *user* and *email*:
```bash
git config --global user.name "Frédéric LAUDARIN"
git config --global user.email "frederic.laudarin@supergrid-institute.com"
```


## Clone repositories

### Clone a branch that is different from the default one

For cloning the branch `my_branch` of the repository at location `<repo location>` (path or URL) that is different from the default one (remote HEAD) use command `git clone` with option `--branch`:

```bash
$> git clone <repo location> --branch my_branch
```

## Export a local repository to GitLab or GitHub

Create the new project on the server so that there is active URL for HTTP or SSH connection.
Change to the directory of the local repository. If it is not a repository yet then use `git init`.
Add a remote to the repository:
```bash
git add remote origin <URL of the remote>
```
Checkout the branch *master*. Check that the remote was properly added with `git remote -v`. Now push to the branch *master* with the option `--set-upstream` (or `-u`) for creating the new distant branch:
```bash
git push --set-upstream origin master
```

## Remote repository management

### Set remote of a local repository

Add a new remote origin
```bash
$ git remote add origin <URL of the repo>
```

Change the remote origin:
```bash
$ git remote rm origin
$ git remote add origin <URL of the repo>
```

For specifying a new *remote* use command `git push --set-upstream` (or `git push -u`):

```bash
$ git push --set-upstream <path to repository>
```

This command pushes the active branch. For pushing branch add option `--all`. If the remote repository does not exist, it may be created by the hosting server (GitLab, GitHub) by specifying a new target branch.

```bash
$ git push --set-upstream <path to repository> <new target branch>
```

*This operation does not set the remote of the local repository.*

**Example**

```bash
$ git push --set-upstream http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git master

Enumerating objects: 57, done.
Counting objects: 100% (57/57), done.
Delta compression using up to 4 threads
Compressing objects: 100% (55/55), done.
Writing objects: 100% (57/57), 108.05 KiB | 2.77 MiB/s, done.
Total 57 (delta 9), reused 0 (delta 0)
remote:
remote: The private project flaudarin/tea-web-app-proto was successfully created.
remote:
remote: To configure the remote, run:
remote:   git remote add origin http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git
remote:
remote: To view the project, visit:
remote:   http://gitlab.supergrid.com/flaudarin/tea-web-app-proto
remote:
To http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git'.
```

This operation does not set the remote of the local repository. As indicated use command `git remote add` for that purpose.

```bash
$ git remote -v

$ git remote add origin http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git

$ git remote -v

origin  http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git (fetch)
origin  http://gitlab.supergrid.com/flaudarin/tea-web-app-proto.git (push)
```

Eventually synchronize with `git fetch`:

```bash
$ git fetch
From http://gitlab.supergrid.com/flaudarin/tea-web-app-proto
 * branch            master     -> FETCH_HEAD
```

## Branch management

### Create a new branch and change to that branch

Creating a new branch *new_branch* without changing to that branch:

```bash
$ git branch new_branch
```

It is then possible to move the pointer HEAD to the new branch:

```bash
$ git checkout new_branch
```

Doing both at once:

```bash
$ git checkout -b python3_migration
Switched to a new branch 'python3_migration'
```

### Create a new remote branch

If a remote branch does not exist it is possible to create it with `git push` and the option `--set-upstream` (or `-u`):

```bash
$ git push --set-upstream origin new_branch
```
or
```bash
$ git push -u origin new_branch
```

### Change to a new local branch tracking a remote one

First update information from the *remote*:

```bash
$ git fetch --all
```

It is needed to create a local branch that tracks a remote branch. The following command will create a *local branch* tracking a *remote branch* and change locally to the new *local branch*:

```bash
$ git checkout --track origin/daves_branch
```

### Remote branch merging

First, move to the branch that will integrate the merge (*e.g.* branch *master*):

```
$ git checkout master
```

Next merge the distant other branch into the current one (option `--squash` for squashing the commits before merging):

```
$ git merge --squash origin/other-branch
```

Solve the **merge conflicts** by editing the conflicting files and selecting the changes to keep. Next track the files with `git add` and make a commit for closing the conflict.

Eventually push the current branch to the remote repository:
```
$ git push origin master
```

### Rebase a branch

First, keep the local repository up-to-date:

```bash
$ git fetch origin
```

Start rebasing *remote_branch* (*e.g.* origin/remote_branch_name) on *local_branch*. If *local_branch* is not specified then it is the *current branch*:

```bash
$ git rebase remote_branch local_branch
```

Resolve conflict with a text editor, then:

```bash
$ git rebase --continue
```

Remove extra files (e.g. *.orig) created by diff tool:

```bash
$ git clean -i
```

Update the remote branch while bypassing the fast-forward rule:

```bash
$ git push --force-with-lease
```

Or

```bash
$ git pull
$ git push
```

### Solve merge conflicts

Merge conflicts are solved with the command `git mergetool`. A specific tool may be specified with the `--tool` option. For instance the following code uses `tkdiff`:
```
git mergetool --tool=tkdiff
```

The merge tool shows the conflict area in each file sequentially. Suppose the file `transformer.py` has a conflict, the conflict area is divided in two parts:
```python
<<<<<<< HEAD:old/path/to/transformer.py

# Code from the base branch

=======

# Code from the merging branch

>>>>>>> origin/merged_branch:new/path/to/transformer.py
```

Each file must be edited and saved to validate its final state. In the end the delimitation lines with symbols `>`, `=` and `<` must have been removed. 
Once the modification are saved, the *mergetool* must be closed. It will automatically open the next conflicting file.

In case a file was removed in the merging branch then it is asked if the original file must be kept. Answering `m` keeps the file and `k` validate the removal.
```
Deleted merge conflict for 'path/to/deleted/file.py':
  {local}: modified file
  {remote}: deleted
Use (m)odified or (d)eleted file, or (a)bort?
```

Once the conflicts have been solved the merging process is pursued by typing `git merge --continue` (or `git rebase --continue` if rebasing).

It is possible to reset the merging process with the command `git merge --abort` (or `git rebase --abort` if rebasing).

### Suppress a local branch

Use `git branch -d`:
```
$ git branch -d local-branch-to-delete
```
If the branch is not merged, an error message occurs. Replacing -d by -D forces the deletion.

### Suppress a distant branch

A remote branch can be deleted using the syntax `git push <remote-name> :<branch-name>`:
```
$ git push origin :PostgreSQL
```
Remote branch `origin/PostgreSQL` is deleted

An alternative command is:
```
$ git push remote-name --delete branch-name
```
or
```
$ git push remote-name -d branch-name
```
If the branch is not merged, an error message occurs. Replacing -d by -D forces the deletion.

Example:
```
$ git push origin --delete PostgreSQL
```

## Working tree and staging area

### Revert the state of modified files in the working tree

For discarding changes in all *unstaged files* in current working directory use:

```bash
$ git checkout -- .
```

For a specific file use:

```bash
git checkout -- path/to/file/to/revert
```

`--` is here to remove argument [ambiguity](https://git-scm.com/docs/git-checkout#_argument_disambiguation).

### Bring back files from the staging area

The specified staged file (or files) is brought back to the state of modified file in the working tree.

```bash
$ git reset /path/to/the/file
```

### Discard every untracked modification

```
git checkout -- .
```

### Stashing untracked modifications

This operation is useful when changing the current branch while some modifications are not committed. Such an operation requires either to commit the modifications first or stash them. If committing is not the appropriate operation the modifications must be stashed. Stashes can be carried on from branch to branch. If multiple stashes are saved they are organized in *stack*.

Saving a new stash and reverting to the state of the last commit (discarding every modification):
```
$ git stash push -u -m "Comments describing the stash"
```
For stashing directory contents recursively:
```
$ git stash push -u --all -m "Comments describing the stash"
```
The option `-u` (or `--include-untracked`) is necessary for stashing *untracked files*. Otherwise, files must be tracked first with `git add`.

List the existing stashes:
```
$ git stash list
stash@{0}: On CurrentBranchName: Comments describing the stash
```

Getting back modifications from the stash identified as `stash@{0}`:
```
git stash apply
```
Modifications from the stash are restored and the stash is preserved.

Restoring & removing a specific stash in the stash stack:
```
git stash pop <stash identifier>
```

Deleting a given stash:
```
git stash drop <stash identifier>
```

Delete all stashes:
```
git stash clear
```

## Tagging

List existing tags:
```bash
git tag
```

Create an *annotated tag* (tag with a comment):
```bash
git tag -a <label of the tag> -m "Comment describing the tag" [hash of the commit to tag]
```
If the hash is omitted then the last commit is tagged.

**Example**

```
$ git tag -a v0.0 -m "Initial structure of the database"
$ git tag
v0.0
$ git show v0.0
tag v0.0
Tagger: Frederic LAUDARIN <frederic.laudarin@supergrid-institute.com>
Date:   Thu May 16 11:08:38 2019 +0200

Initial structure of the database

commit 7056acff5737cdbd1cf53a927b3062e69afd832b (HEAD -> master, tag: v0.0)
Author: Frederic LAUDARIN <frederic.laudarin@supergrid-institute.com>
Date:   Thu May 16 10:54:16 2019 +0200
...
```

## Edit the last commit

Suppose additional modifications must be added to the last commit. Edit files and add the modifications with `git add`. Next update the last commit with `git commit --amend`:
```bash
$ git commit --amend -m "New comment"
```
For keeping the comment of the last commit use instead:
```bash
$ git commit --amend --no-edit
```

If the commit was pushed to the remote repository before being amended then Git security will prevent from pushing the last changes. Actually the remote commit and the local amended one are considered on different branches as the local branch and the remote one diverge. A `git pull` is required first, it will merge the last commit on the remote in the last local commit of the branch.
```
$ git pull
Merge made by the 'recursive' strategy.
```
Eventually make a `git push` for updating the remote.

## List files changed between two commits

Use `git diff --name-only`.

|  |  |
|--|--|
|Comparing a commit with the one `HEAD` is located | `git diff --name-only <commit hash>`|
|Comparing two commits|`git diff --name-only <commit #ref hash> <commit #comp hash>`|
