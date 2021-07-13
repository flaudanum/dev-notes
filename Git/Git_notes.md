<style>
h1 {
  text-decoration: underline overline;
  color: #b52727;
  font-size: 3em;
}
h2 {
  color: #d82c2c;
}
h3 {
  color: #de4e4e;
}
</style>

# Git notes

- [Git notes](#git-notes)
  - [Configuration](#configuration)
  - [Clone repositories](#clone-repositories)
    - [Clone a branch that is different from the default one](#clone-a-branch-that-is-different-from-the-default-one)
  - [Export a local repository to GitLab or GitHub](#export-a-local-repository-to-gitlab-or-github)
  - [Remote repository management](#remote-repository-management)
    - [Set remote of a local repository](#set-remote-of-a-local-repository)
    - [Update local branches with remote](#update-local-branches-with-remote)
  - [Revert to a previous commit](#revert-to-a-previous-commit)
  - [Reset last commit](#reset-last-commit)
  - [Branch management](#branch-management)
    - [Create a new branch and change to that branch](#create-a-new-branch-and-change-to-that-branch)
    - [Create a new remote branch](#create-a-new-remote-branch)
    - [Change to a new local branch tracking a remote one](#change-to-a-new-local-branch-tracking-a-remote-one)
    - [Track a remote branch which is not in LOCAL](#track-a-remote-branch-which-is-not-in-local)
    - [Finding what branch a git commit came from](#finding-what-branch-a-git-commit-came-from)
    - [Remote branch merging](#remote-branch-merging)
    - [Rebase a branch](#rebase-a-branch)
    - [Solve merge conflicts with `mergetool`](#solve-merge-conflicts-with-mergetool)
    - [Solve merge conflicts (direct edition)](#solve-merge-conflicts-direct-edition)
    - [Accept all changes](#accept-all-changes)
    - [Merge / Rebase: local, remote, ours, theirs](#merge--rebase-local-remote-ours-theirs)
    - [Suppress a local branch](#suppress-a-local-branch)
    - [Suppress a distant branch](#suppress-a-distant-branch)
  - [Working tree and staging area](#working-tree-and-staging-area)
    - [Revert the state of modified files in the working tree](#revert-the-state-of-modified-files-in-the-working-tree)
    - [Bring back files from the staging area](#bring-back-files-from-the-staging-area)
    - [Discard every untracked modification](#discard-every-untracked-modification)
    - [Stashing untracked modifications](#stashing-untracked-modifications)
  - [Tagging](#tagging)
    - [Create a new tag](#create-a-new-tag)
    - [Push local tags to origin](#push-local-tags-to-origin)
    - [Remove a tag](#remove-a-tag)
  - [Edit the last commit](#edit-the-last-commit)
  - [Analyze changes between commits](#analyze-changes-between-commits)
    - [List files changed between two commits](#list-files-changed-between-two-commits)
    - [Modification of a file](#modification-of-a-file)

## Configuration

List configuration:

```
git config --list
```

Set the global settings for properties _user_ and _email_:

```
git config --global user.name "Frédéric LAUDARIN"
git config --global user.email "friedrich.laudanum@my-email.com"
```

## Clone repositories

### Clone a branch that is different from the default one

For cloning the branch `my_branch` of the repository at location `<repo location>` (path or URL) that is different from the default one (remote HEAD) use command `git clone` with option `--branch`:

```
$> git clone <repo location> --branch my_branch
```

## Export a local repository to GitLab or GitHub

Create the new project on the server so that there is active URL for HTTP or SSH connection.
Change to the directory of the local repository. If it is not a repository yet then use `git init`.
Add a remote to the repository:

```
git add remote origin <URL of the remote>
```

Checkout the branch _master_. Check that the remote was properly added with `git remote -v`. Now push to the branch _master_ with the option `--set-upstream` (or `-u`) for creating the new distant branch:

```
git push --set-upstream origin master
```

## Remote repository management

### Set remote of a local repository

Add a new remote origin

```
$ git remote add origin <URL of the repo>
```

Change the remote origin:

```
$ git remote rm origin
$ git remote add origin <URL of the repo>
```

For specifying a new _remote_ use command `git push --set-upstream` (or `git push -u`):

```
$ git push --set-upstream <path to repository>
```

This command pushes the active branch. For pushing branch add option `--all`. If the remote repository does not exist, it may be created by the hosting server (GitLab, GitHub) by specifying a new target branch.

```
$ git push --set-upstream <path to repository> <new target branch>
```

_This operation does not set the remote of the local repository._

**Example**

```
$ git push --set-upstream http://gitlab.my-company.com/flaudanum/web-app-proto.git master

Enumerating objects: 57, done.
Counting objects: 100% (57/57), done.
Delta compression using up to 4 threads
Compressing objects: 100% (55/55), done.
Writing objects: 100% (57/57), 108.05 KiB | 2.77 MiB/s, done.
Total 57 (delta 9), reused 0 (delta 0)
remote:
remote: The private project flaudanum/web-app-proto was successfully created.
remote:
remote: To configure the remote, run:
remote:   git remote add origin http://gitlab.my-company.com/flaudanum/web-app-proto.git
remote:
remote: To view the project, visit:
remote:   http://gitlab.my-company.com/flaudanum/web-app-proto
remote:
To http://gitlab.my-company.com/flaudanum/web-app-proto.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'http://gitlab.my-company.com/flaudanum/web-app-proto.git'.
```

This operation does not set the remote of the local repository. As indicated use command `git remote add` for that purpose.

```
$ git remote -v

$ git remote add origin http://gitlab.my-company.com/flaudanum/web-app-proto.git

$ git remote -v

origin  http://gitlab.my-company.com/flaudanum/web-app-proto.git (fetch)
origin  http://gitlab.my-company.com/flaudanum/web-app-proto.git (push)
```

Eventually synchronize with `git fetch`:

```
$ git fetch
From http://gitlab.my-company.com/flaudanum/web-app-proto
 * branch            master     -> FETCH_HEAD
```

### Update local branches with remote

Suppose a remote branch was deleted. The command `git fetch --all` will not update the database on the remote repository state in the local repository. Such update is triggered with:

```
$ git fetch --prune
```

It removes any remote-tracking references that no longer exist on the remote.

## Revert to a previous commit

This operation can be performed with `git reset`. Use option `--soft` to keep changed files in the working tree or `--hard` to erase every changes.

```
$ git reset --soft <commit>
```

## Reset last commit

The last commit is undone and the associated changes are tracked.

```
$ git reset --soft "HEAD^"
```

This can be used to merge the last commit with the previous:

```
$ git reset --soft "HEAD^"
$ git commit --amend
```

## Branch management

### Create a new branch and change to that branch

Creating a new branch _new_branch_ without changing to that branch:

```
$ git branch new_branch
```

It is then possible to move the pointer HEAD to the new branch:

```
$ git checkout new_branch
```

Doing both at once:

```
$ git checkout -b python3_migration
Switched to a new branch 'python3_migration'
```

### Create a new remote branch

If a remote branch does not exist it is possible to create it with `git push` and the option `--set-upstream` (or `-u`):

```
$ git push --set-upstream origin new_branch
```

or

```
$ git push -u origin new_branch
```

### Change to a new local branch tracking a remote one

First update information from the _remote_:

```
$ git fetch --all
```

It is needed to create a local branch that tracks a remote branch. The following command will create a _local branch_ tracking a _remote branch_ and change locally to the new _local branch_:

```
$ git checkout --track origin/daves_branch
```

### Track a remote branch which is not in LOCAL

When you clone a repository you do not automatically track every remote branch. In order to see a complete list of current remote branches:

```
$ git fetch --all --prune
$ git branch --all
  remotes/origin/development
  remotes/origin/main
  remotes/origin/my-branch
```

Next to start tracking locally the remote branch `my-branch` and change to it:

```
git checkout --track origin/my-branch
```

### Finding what branch a git commit came from

```
$ git branch --contains <commit hash>
```

### Remote branch merging

First, move to the branch that will integrate the merge (_e.g._ branch _master_):

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

```
$ git fetch origin
```

Start rebasing _remote_branch_ (_e.g._ origin/remote*branch_name) on \_local_branch*. If _local_branch_ is not specified then it is the _current branch_:

```
$ git rebase remote_branch local_branch
```

Resolve conflict with a text editor, then:

```
$ git rebase --continue
```

Remove extra files (e.g. \*.orig) created by diff tool:

```
$ git clean -i
```

Update the remote branch while bypassing the fast-forward rule:

```
$ git push --force-with-lease
```

Or

```
$ git pull
$ git push
```

### Solve merge conflicts with `mergetool`

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
Once the modification are saved, the _mergetool_ must be closed. It will automatically open the next conflicting file.

In case a file was removed in the merging branch then it is asked if the original file must be kept. Answering `m` keeps the file and `k` validate the removal.

```
Deleted merge conflict for 'path/to/deleted/file.py':
  {local}: modified file
  {remote}: deleted
Use (m)odified or (d)eleted file, or (a)bort?
```

Once the conflicts have been solved the merging process is pursued by typing `git merge --continue` (or `git rebase --continue` if rebasing).

Next remove extra files (e.g. \*.orig) created by diff tool:

```
$ git clean -i
```

Update the remote branch while bypassing the fast-forward rule:

```
$ git push --force-with-lease
```

Or

```
$ git pull
$ git push
```

It is possible to reset the merging process with the command `git merge --abort` (or `git rebase --abort` if rebasing).

### Solve merge conflicts (direct edition)

Instead of using the `mergetool` option, make a `git status` to see the files where there are conflicts. Those files have status of `modified in both`. Edit the files and solve conflicts indicated in the areas delimited with symbols `>`, `=` and `<` as in the previous section.

Check the validity of your code (run tests). Then add your changes to the staging area (`git add --all`) and make `git merge --continue`. An editor should open for you to write a message for the merge commit.

### Accept all changes

For example if you already know that you want to accept ALL the changes in a file on your **local** branch and discard the other branch’s edits. Instead of opening the file, finding the conflict regions, then making the appropriate changes, you can more succinctly prefer your changes with the following command:

```
git checkout --ours .
```

After you’ve finished, stage the the conflict files, and continue your rebase:

```
git add /path/to/conflict_file.rb
git rebase --continue
```

### Merge / Rebase: local, remote, ours, theirs

```
git checkout A
git rebase   B    # rebase A on top of B
```

- local is B (rebase onto),
- remote is A

And:

```
git checkout A
git merge    B    # merge B into A
```

- local is A (merge into),
- remote is B

A rebase switches **ours** (current branch before rebase starts) and **theirs** (the branch on top of which you want to rebase).

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

For discarding changes in all _unstaged files_ in current working directory use:

```
$ git checkout -- .
```

For a specific file use:

```
git checkout -- path/to/file/to/revert
```

`--` is here to remove argument [ambiguity](https://git-scm.com/docs/git-checkout#_argument_disambiguation).

### Bring back files from the staging area

The specified staged file (or files) is brought back to the state of modified file in the working tree.

```
$ git reset /path/to/the/file
```

### Discard every untracked modification

```
git checkout -- .
```

### Stashing untracked modifications

This operation is useful when changing the current branch while some modifications are not committed. Such an operation requires either to commit the modifications first or stash them. If committing is not the appropriate operation the modifications must be stashed. Stashes can be carried on from branch to branch. If multiple stashes are saved they are organized in _stack_.

Saving a new stash and reverting to the state of the last commit (discarding every modification):

```
$ git stash push -u -m "Comments describing the stash"
```

For stashing directory contents recursively:

```
$ git stash push -u --all -m "Comments describing the stash"
```

The option `-u` (or `--include-untracked`) is necessary for stashing _untracked files_. Otherwise, files must be tracked first with `git add`.

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

### Create a new tag

List existing tags:

```
git tag
```

Create an _annotated tag_ (tag with a comment):

```
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
Tagger: Friedrich LAUDANUM <friedrich.laudanum@my-email.com>
Date:   Thu May 16 11:08:38 2019 +0200

Initial structure of the database

commit 7056acff5737cdbd1cf53a927b3062e69afd832b (HEAD -> master, tag: v0.0)
Author: Friedrich LAUDANUM <friedrich.laudanum@my-email.com>
Date:   Thu May 16 10:54:16 2019 +0200
...
```

### Push local tags to origin

Pushing the tags to the remote repository with option `--tags`:

```
$ git push --tags
```

**Beware**:

- This command only pushes tags not commits.
- Do not push tags associated to commits that are not pushed

In the second situation, follow these steps:

- Remove the tag in the local branch (`git tag --delete`)
- Remove the tag in the remote branch (e.g. with GitLab's UI)
- re-push the tags

And Robert's your father's brother.

### Remove a tag

```
$ git tag --delete <tag name>
```

## Edit the last commit

Suppose additional modifications must be added to the last commit. Edit files and add the modifications with `git add`. Next update the last commit with `git commit --amend`:

```
$ git commit --amend -m "New comment"
```

For keeping the comment of the last commit use instead:

```
$ git commit --amend --no-edit
```

If the commit was pushed to the remote repository before being amended then Git security will prevent from pushing the last changes. Actually the remote commit and the local amended one are considered on different branches as the local branch and the remote one diverge. A `git pull` is required first, it will merge the last commit on the remote in the last local commit of the branch.

```
$ git pull
Merge made by the 'recursive' strategy.
```

Eventually make a `git push` for updating the remote.

## Analyze changes between commits

### List files changed between two commits

Use `git diff --name-only`.

|                                                   |                                                               |
| ------------------------------------------------- | ------------------------------------------------------------- |
| Comparing a commit with the one `HEAD` is located | `git diff --name-only <commit hash>`                          |
| Comparing two commits                             | `git diff --name-only <commit #ref hash> <commit #comp hash>` |

### Modification of a file

For viewing the modification of a file between two commits, specify the commits' hashes followed by the file path. <i style="color:grey">What about a file which path has been modified between the commits?</i>

```
git diff --name-only <commit #ref hash> <commit #comp hash> /path/to/the/file
```

The code of the modified parts are indicated as follows:

<span style="color:green"><tt>+ code from commit #comp</tt></span><br/>
<span style="color:red"><tt>- code from commit #ref</tt></span><br/>

Example:

```
$ git diff c34076e 0d477d4 my_app/controller/data_reader.py
```
