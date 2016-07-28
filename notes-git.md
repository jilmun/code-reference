---
layout: page
title: git
---

# Git Reference

-----

## Basics


**Configure Git**

* `git config --global user.name "Your Name"` - set user name
* `git config --global user.email "user@domain.com"` - set email
* `cd /path/to/repository` - set directory path
* `git config --global core.editor /path/to/editor` - set default editor
* `git config --global color.ui auto` - turn on colors when possible


**Create Repository for New Project**

* `mkdir some-repository`
* `cd some-repository`
* `git init`


**Create Repository for Existing Project**

* `cd /path/to/some/directory`
* `git init`


**Clone Repository**

* `git clone https://github.com/username/repo.git`
* `git clone git://github.com/username/repo.git`
* `git clone git://github.com/username/repo.git cloned-repo` - clone to specific path
* `git clone --depth 50 some-repository` - shallow clone with last 50 commits


**Stage Files**

* `git add .` - add all files
* `git add -A` or `git add --all` - add all files not explicitly ignored
* `git add -u` or `git add --update` - add changed tracked files, does not add new files
* `git add myfile.*` - add all files beginning with "myfile."
* `git add -p` or `git add --patch` - specifically choose which changes to commit, select 'y' or 'n' for each change
* git does not track empty directories (as of v1.7) but adding an empty dot file will force tracking (i.e. ".include_in_git")


**Check Status and Commit Changes**

* `git status`
* `git commit -m "initial commit"` 


**Ignore Files**

* add files to .gitignore: i.e. `cache`, `*.RData`
* `git config --global core.excludesfile ~/.gitignore` - set up global excludes file in .gitignore in home directory


**Undo Changes**

* `git reset HEAD -- myfile.R` - unstage myfile.R
* `git checkout -- myfile.R` - undo uncommitted changes to myfile.R


**Share Changes**

* `git fetch reponame` - get changes from remote repository
* `git pull reponame` - pull from remote repository
* `git pull origin` - pull and rebase local changes on top of remote changes
* `git pull --rebase origin remotebranch`
* `git push remotename branchname` - push changes to remote repository


**Create Branches**

* `git branch somebranchname optionalcommitIDorTag` - create branch
* `git checkout somebranchname` - switch to branch
* `git checkout -b somebranchname` - create and switch to branch
* `git branch --track newbranchname` - default tracking for remote branches
* `git branch --no-track new branchname` - turn tracking off


**View Branches**

* `git branch` - view local branches
* `git branch -r` - view remote branches
* `git branch -a` - view all branches
* `git branch --merged` - view merged branches in current branch
* `git branch --no-merged` - view unmerged branches
* `git branch --contains somecommitID` - view branches with some commitID


**Merge Branches**

* `git checkout master`
* `git merge developmentbranch` - merge new branch into master
* `git merge --no-commit developmentbranch` - merge without commit
* `git merge --no-ff developmentbranch` - force merge commit, no fast forward
* `git merge -m "custom commit message" developmentbranch` - merge with custom commit message
* `git rebase master` - update development branch with new master commits
* `git rebase commitIDorTag` - update parent for development branch
* `git reset --hard ORIG_HEAD` - undo a rebase
* `git branch -d mergedbranch` - delete unused merged branch
* `git branch -D unmergedbranch` - delete experimental unmerged branch


**Tag Commits**

* `git tag` - list all tags, remote and local treated the same
* `git tag v1.0` - create tag on latest commit in current branch


**View Log**

* `git log` - see reverse chronological list of commits
* `git log --oneline` - see shortened commit ID and messages in one line
* `git log -5` - see last 5 commits
* `git log -1 -p HEAD` - see changes in most recent commit
* `git log -- filename` - see log for single file
* `git log --after="7 days"` - see commits in last week
* `git log --grep="some regular expression"` - view log entries with some string


**View Changes**

* `git diff` - diff current working tree and staging area
* `git diff --stages` - diff staged changes and repository
* `git diff HEAD` - diff working tree and repository
* `git diff firstcommitIDorTag secondcommitIDorTag` - diff two commits


**Other Commands**

* `git mv README.md README.txt` or `git mv README.md wiki/` - rename or move README file
* `git rm -- outdated.txt` - remove file
* `git rm -r -- outdated/` - remove directory
* `git remote add branchname webURL` - add new remote repository
* `git remote rm branchname` - remove remote repository
* `git pull origin remotebranch:localbranch` - pull remote branch into local branch
* `git push origin remotebranch:localbranch` - push local branch into remote branch
* `git push origin :beta` - delete remote branch called beta (replaces with empty branch)
* `git revert --no-edit commitID` - revert commit with default revert commit message
* `git reset --soft HEAD^` - reset last commit and stage changes
* `git gc` - run garbage collection by default removes orphans older than 2 weeks
* `git gc --prune="1 week"` - remove loose objects older than 1 week


-----

## Miscellaneous

### Working with Forks

Reset origin from original upstream repository to personal fork

`git remote set-url origin https://github.com/username/myrepo.git`

Add original upstream repository as another remote and merge upstream commits into fork

* `git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git`
* `git pull https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git BRANCH_NAME`
* `git push origin master`

### Squash Commits

Example of squashing last 3 commits with message "squash commits"

`git reset --soft HEAD~3 && git commit -m "squash commits"`

Force push

`git push -f`


### Untrack Changes

**In a directory**

This command will cause git to untrack your directory and all files under it without actually deleting them:

`git rm -r --cached <your directory>`

* The `-r` option causes the removal of all files under your directory.
* The `--cached` option causes the files to only be removed from git's index, not your working copy. By default git rm <file> would delete <file>.

**For a specific file**

Use the following command:

`git update-index --assume-unchanged FILE_NAME`

To track the changes again use this command:

`git update-index --no-assume-unchanged FILE_NAME`


### Merging Branch

After the merge, it's safe to delete the branch:

`git branch -d branch1`

Additionally, git will warn you (and refuse to delete the branch) if it thinks you didn't fully merge it yet. If you forcefully delete a branch (with git branch -D) which is not completely merged yet, you have to do some tricks to get the unmerged commits back though (see below).

There are some reasons to keep a branch around though. For example, if it's a feature branch, you may want to be able to do bugfixes on that feature still inside that branch.

If you also want to delete the branch on a remote host, you can do:

`git push origin :branch1`

This will forcefully delete the branch on the remote (this will not affect already checked-out repositiories though and won't prevent anyone with push access to re-push/create it).
