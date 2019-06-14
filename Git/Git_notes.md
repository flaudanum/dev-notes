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
    - [Remote branch merging](#remote-branch-merging)
  - [Rebase a branch](#rebase-a-branch)
  - [Working tree and staging area](#working-tree-and-staging-area)
    - [Revert the state of modified files in the working tree](#revert-the-state-of-modified-files-in-the-working-tree)
    - [Bring back files from the staging area](#bring-back-files-from-the-staging-area)
    - [Discard every untracked modification](#discard-every-untracked-modification)
    - [Stashing untracked modifications](#stashing-untracked-modifications)
  - [Tagging](#tagging)


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

### Remote branch merging
First, move to the branch that will integrate the merge (*e.g.* *master*):

```bash
$ git checkout master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
```

Then merge the remote branch *origin/branch_name* within:

```bash
$ git merge origin/branch_name
```

Then push to origin

```bash
$ git push origin master
```

## Rebase a branch

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

This operation is useful when changing the current branch while some modifications are not committed. Such an operation requires either to comit the modifications first or stash them. If committing is not the appropriate operation the modifications must be stashed. Stashes can be carried on fromn branch to branch. If multiple stashes are saved they are organized in *stack*.

Saving a new stash and reverting to the state of the last commit (discarding every modification):
```
$ git stash save "Comments describing the stash"
```

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