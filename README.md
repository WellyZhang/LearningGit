# Learning Git

## Introduction

This repo contains the notes I made while learning the Git command-line tool from [Xuefeng's blog](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) and [Git Tutorial](https://www.atlassian.com/git/tutorials/). Git is an advanced distributed version control tool that enables release, cooperation and evolution. Mastering Git really helps boost the productivity and maintain the software.

A Git command cheatsheet by Tower is also provided here for your reference.

## Basics

### Install Git

To use Git, first open your terminal and type 
```
git
```
to check whether you've installed Git. If not, 
```
sudo apt-get install git
```
to install under Ubuntu or visit [this](https://git-scm.com/) for a Windows version.

### Configure Git

The Git global configuration file is stored in ~/.gitconfig while the local repo configuration file resides in \<repo\>/.git/config. You could either change your configuration in a text editor or use
```
git config --global user.name "<YourName>"
git config --global user.email "<email@example.com>"
```

### Initialization

To initialize a Git repo, you could either initialize it locally by
```
git init
```
just note to use UTF-8 encoding without BOM under Windows. Or use 
```
git init <directory>
git init --bare <directory>.git
```
The ```--bare``` flag creates a repository that doesn’t have a working directory, making it impossible to edit files and commit changes in that repository. Central repositories should always be created as bare repositories because pushing branches to a non-bare repository has the potential to overwrite changes. Think of --bare as a way to mark a repository as a storage facility, opposed to a development environment.

A more common way to craete local copies is
```
git clone <protocol>
```
where the protocol could be either https or git. Currently, git is the fastest protocol supported but https is safer.

To create an initial commit of the current directory, use the following two commands:
```
git add .
git commit
```

### Modification

After changing files, including adding, modifying and removing, 
```
git add <file>
git add <directory>
...
git commit -m "<note>" (usually present tense)
```
to commit the changes. If many files are changed, could use
```
git add --all
```
Remember, every time files are changed, git add and git commit.

```git add``` command moves changes from the working directory to the Git staging area. The staging area is where you prepare a snapshot of a set of changes before committing them to the official history. ```git commit``` takes the staged snapshot and commits it to the project history. Combined with git add, this process defines the basic workflow for all Git users.

The --amend option in commit combines the staged changes with the previous commit and replace the previous commit with the resulting snapshot. Running this when there is nothing staged lets you edit the previous commit's message without altering its snapshot.

Note that Git repo consists of the working directory that holds all your files, the stage that caches your modifications and HEAD that tracks your commit history.  

To remove a file, first delete it in the repo then
```
git rm <filename>
```
and commit the change. 

Directly typing
```
git checkout -- <filename>
```
will replace the file with the one in HEAD.

The git checkout command serves three distinct functions: checking out files, checking out commits, and checking out branches. How to check out files and commits is shown here with branches addressed later.

Checking out a commit makes the entire working directory match that commit. This can be used to view an old state of your project without altering your current state in any way. 
```
git checkout <commit>/<tag>/HEAD <file> (HEAD is the current commit)
```
will turn \<file\> into a copy from \<commit\>/\<tag\>/HEAD and add it to the staging area. If you leave \<file\>, the working directory is matched to the specific commit/tag/HEAD. 

Note that checking out old commits is a read-only operation -- nothing you do in here will be saved in your repository. During the normal course of development, the HEAD usually points to master or some other local branch, but when you check out a previous commit, HEAD no longer points to a branch -- it points directly to a commit. This is called a "detached HEAD" state. But checking out an old file does affect the current state of your repository (staged). You can re-commit the old version in a new snapshot as you would any other file.

###  Status and Logging

To check the staus of the repo, type
```
git status
```
This will show which files are staged, unstaged and untracked. A new file starts as untracked; after added it is staged. A changed existant file is in the state of unstaged; git add will stage it to be committed.

To see how a file is changed
```
git diff <file>
```

The git log command displays committed snapshots. While git status lets you inspect the working directory and the staging area, git log only operates on the committed history. The -p option gives a detailed view. And git reflog keeps track of the updates of tips of branches.

### Revert, Reset and Clean

```
git revert <commit>
```
generates a new commit that undoes all of the changes introduced in <commit>, then apply it to the current branch. It does not remove the commit from the project history.

Reset "revert"s back to the previous state of a project by removing all subsequent commits. When you undo with git reset (and the commits are no longer referenced by any ref or the reflog), there is no way to retrieve the original copy -- it is a permanent undo.

```
git reset [--hard] <file>/<commit> e.g. HEAD^   (the last version)
                                        HEAD^^  (the second to last)
                                        HEAD~10 (the 10th to last)
```
Without --hard, this command resets the staging area to match the most recent commit, but leaves the working directory unchanged; with it, the working directory is overwritten.

Note that ~ is useful for making relative references to the parent of a commit. For example, 3157e~1 refers to the commit before 3157e, and HEAD~3 is the great-grandparent of the current commit.

As soon as you add new commits after the reset, Git will think that your local history has diverged from origin/master and merge is needed. 

For public repos, never use reset.

The git clean command removes untracked files from your working directory. This is not undoable. This command is often executed in conjunction with ```git reset --hard```. Remember that resetting only affects tracked files, so a separate command is required for cleaning up untracked ones. Combined (with -f option), these two commands let you return the working directory to the exact state of a particular commit.

### Stage

Stage is a temporay space for changes into files. git add stores the changes instead of files into the Stage; git commit pushes the changes in Stage to the current branch.

To track the diffrence between the working directory and the branch
```
git diff HEAD -- <filename>
```    

Git also allows you to unstage. To discard changes in the working directory and return to a stage of the nearest git add or git commit
```
git checkout -- <filename>
```
If you have already git add
```
git reset HEAD <filename>
```
to unstage.

### Distributed Git

To use GitHub for distributed Git, first obtain your RSA key by following the steps

1. cd to .ssh
2. call ```ssh-keygen -t rsa -C "youremail@example.com"```
3. Key pressing Enter
4. Log into the GitHub account setting web and copy the contents of .ssh/id_rsa.pub to ssh key
    
To add a remote repo, 
```
git remote add origin https://github.com/WellyZhang/LearningGit.git
```
Note that origin is the default name of the remote repo.
Then 
```
git push origin master
```

### Branching

The main branch is called master and is the default working space.There is actually a HEAD pointer that points to the current version of master. Since HEAD is just a pointer, it could point to any other branch. You can think of a branch as a brand-new working directory, staging area, and project history, or an independent line of development for the repo.

When you create a branch, all Git needs to do is create a new pointer.  

To create a new brach (based on the current one) and navigate to it
```
git branch <branchname>
git checkout <branchname>
```
or type
```
git checkout -b <branchname>
```
to combine the two. Now changes are tracked in the new branch and master won't be affected.

Note that to create a new branch based on another branch,
```
git branch <new-branch> <existing-branch>
```

Other commands related to branching are
* ```git branch``` checks the current branch and lists all
* ```git merge <branchname>``` merges the branch to the current branch
* ```git branch -d/D <branchname>``` deletes the branch (-D for force delete)
* ```git merge --no-ff -m "<note>" <branchname>``` does not delete the branch when merging

Note the easist merging mode is fast-forward: it simply moves the current branch tip to the target branch tip. 

When conflicts occur when you merge two branches, mannually resolve them by checking the repo's status and the branch merge graph by
```
git log --graph --pretty=oneline --abbrev-commit
```

When adding a new feature to your program, create a new branch for it so that code in the Master branch is not tourched.

### Stash

```git stash``` command takes your uncommitted changes (both staged and unstaged), saves them away for later use, and then reverts them from your working copy. Note that the stash is local to your Git repository; stashes are not transferred to the server when you push. To reapply the changes, 
```
git stash pop (this removes the changes in the stash)
git stash apply (this keeps the changes in the stash)
```

```git stash drop stash@{...}``` and ```git stash clean``` delete stashes.

By default, stash only stashes staged changes and unstaged changes made to tracked files, but not to untracked files and ignored files. To stash untracked files, add -u; to stash ignored files, add -a.

You're not limited to have a single stash. ```git stash list``` will list all the stashes you have. Once you see the stash list, you will immediately realize the importance of attached messages. Luckily, Git allows you to annotate your stashes by
```
git stash save "<message>"
```

Note that pop reapplies the latest stash, i.e. stash@{0}. To choose mannually, 
```
git stash pop stash@{...}
```

Stash also allows you to view differences by stash show. Add -p for a full view.

You can also choose to stash just a single file, a collection of files, or individual changes from within files. If you pass the -p option (--patch) to git stash, it will iterate through each changed "hunk" in your working copy and ask whether you wish to stash it.

If the changes on your branch diverge from the changes in your stash, you may run into conflicts when popping or applying your stash. Instead, you can use git stash branch to create a new branch to apply your stashed changes to. 
```
git stash branch add-style stash@{...}
```
This checks out a new branch based on the commit that you created your stash from, and then pops your stashed changes onto it.

### Tagging

Tagging is an alias of a commit ID. To give a tagging, navigate to the branch 
```
git tag lists
```
to list all the branch, 
```
git tag <tag>
```
gives a new tag to the latest commit;
```
git tag <tag> <commit>
```
gives a tag to a specified commit;
```
git show <tag>
```
returns details of a commit with a corresponding tag name;
```
git tag -a/s <tag> -m "<notes>"
```
attaches the notes;
```
git tag -d <tag>
```
deletes the local tag;
```
git push origin <tag>
```
or
```
git push origin --tags
```
pushes the commit with the tag name. TO delete a remote repo, use
```
git tag -d <tag>
```
to delete locally and then
```
git push origin :refs/tags/<tag>
```

### Cooperation

Before cooperation, ```git remote``` shows info about the remote repo and ```git remote -v``` gives details of the repo. When you want to push to a branch, use
```
git push origin <branchname>
```
where again origin is the name of the remote repo.

Like ```git checkout -b dev origin/dev``` just opens the new branch, when there are conflicts, first git pull,  
```
git checkout -b <branchname> origin/<branchname>
```
then
```
git pull --set-upstrem <branchname> origin/<branchname>
```
to establish connection of the local and remote repos. After conflicts are resolved, git push.
  
To contribute to an existing project, first fork the repo, then clone, add something, push to your own repo and send pull request to contribute.

Rebasing is commonly used in open-sourced projects. It is the process of moving a branch to a new base commit. 
```
git rebase <base> (from another branch and then merge this branch)
```
rebases the current branch onto <base>, which can be any kind of commit reference. It results in a fast-forward merge and a perfectly linear history and is a common way to integrate upstream changes into your local repository. It's like saying "I want to base my changes on what everybody has already done."

A -i option in rebase begins an interactive rebasing session. This gives you the opportunity to alter individual commits in the process.

### gitignore

.gitignore tracks the file not to add to the repo. This file should include but not limited to system-generated cache, intermediate and executable files or ones containing sensitive info.

.gitignore uses globbing patterns to match against file names, # to include comments and \ to escape. There could be multiple ignore files in different levels of your repo.

If you want to ignore a file that you've committed in the past, you'll need to delete the file from your repository and then add a .gitignore rule for it. Using the --cached option with git rm means that the file will be deleted from your repository, but will remain in your working directory as an ignored file.

To add ignored files, use
```
git add -f <filename> to force add
```

The following command shows why a file is ignored or not:
```
git check-ignore -v <filaname>
```

## Collaboration

### Sync

git remote lists the remote connections you have to other repositories. A -v option will include URLs of each connection. The remote command also has add, rm and rename options. When you clone a repository with git clone, it automatically creates a remote connection called origin pointing back to the cloned repository. 

Fetching is what you do when you want to see what everybody else has been working on. Since fetched content is represented as a remote branch, it has absolutely no effect on your local development work. The command is formatted as:
```
git fetch <remote> [<branch>]
```

Here remote branches are like local branches, except they represent commits from somebody else’s repository. You can check out a remote branch just like a local one, but this puts you in a detached HEAD state (just like checking out an old commit). You can think of them as read-only branches. To show remote branches (named after <remote>/<branch>), add -r in the git branch command. If changes in the remote branches are approved, you can merge it with your master branch. This synchronizes the local repo.

git pull combines the fetch and merge workflow. git pull <remote> is equivalent to git fetch <remote> and git merge origin/<current-branch>. Its --rebase option moves the local changes onto the top of latest update.

git push transfers commits from your local repository to a remote repo, formatted as 
```
git push <remote> <branch>/--all [--force]
```
with the --force option results in push even when non-fast-forward merge.

### Resolvin Conflicts in Merge

Git has two ways to accomplish merge: fast-forward merge and 3-way merge.

A fast-forward merge can occur when there is a linear path from the current branch tip to the target branch. Instead of "actually" merging the branches, all Git has to do to integrate the histories is move (i.e., "fast forward") the current branch tip up to the target branch tip. 

However, a fast-forward merge is not possible if the branches have diverged. When there is not a linear path to the target branch, Git has no choice but to combine them via a 3-way merge. 3-way merges use a dedicated commit to tie together the two histories. The nomenclature comes from the fact that Git uses three commits to generate the merge commit: the two branch tips and their common ancestor.

But if two branches both change the same file, Git won't be able to figure out which version to use. This situation requires manual resolve. git status to check the files where conflicts occur, fix up the merges and git add.

When the remote repo is modified before you push, it's suggested you push with the --rebase option. But if the rebase process generates some conflicts, Git will pause at this specific commit and wait for you to resolve it manually. Once the conflict is removed, you can git add and git rebase --continue.

## Advanced Tips

### Reset, Checkout and Revert

On the commit-level, resetting is a way to move the tip of a branch to a different commit. This can be used to remove commits from the current branch. By using reset, you actually throw away commits. git reset has three options: --soft only alters the project history; --mixed leaves only the working directory unchanged and is the defaulyt; --hard updates all the three components in a git repo.

You can also check out arbitrary commits by passing in the commit reference instead of a branch. This does the exact same thing as checking out a branch: it moves the HEAD reference to the specified commit. But it results in a detached HEAD state.

Reverting undoes a commit by creating a new commit. This is a safe way to undo changes, as it has no chance of re-writing the commit history. Contrast this with git reset, which does alter the existing commit history. For this reason, git revert should be used to undo changes on a public branch, and git reset should be reserved for undoing changes on a private branch.

You can also think of git revert as a tool for undoing committed changes, while git reset HEAD is for undoing uncommitted changes.

The git reset and git checkout commands also accept an optional file path as a parameter. This dramatically alters their behavior. 

When invoked with a file path, git reset updates the staged snapshot to match the version from the specified commit. The --soft, --mixed, and --hard flags do not have any effect on the file-level version of git reset, as the staged snapshot is always updated, and the working directory is never updated. 

Checking out a file is similar to using git reset with a file path, except it updates the working directory instead of the stage. Unlike the commit-level version of this command, this does not move the HEAD reference, which means that you won’t switch branches. If you stage and commit the checked-out file, this has the effect of “reverting” to the old version of that file. Note that this removes all of the subsequent changes to the file, whereas the git revert command undoes only the changes introduced by the specified commit.

