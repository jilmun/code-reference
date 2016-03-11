---
layout: page
title: Git Basics
---

# Git Reference

### Rename File or Folder

`git mv README README.md`

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