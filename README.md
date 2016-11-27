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

The Git global configuration file is stored in ~/.gitconfig while the local repo configuration file resides in <repo>/.git/config. You could either change your configuration in a text editor or use
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
git checkout <commit>/<tag>/HEAD <file>
```
will turn <file> into a copy from <commit>/<tag>/HEAD and add it to the staging area. If you leave <file>, the working directory is matched to the specific commit/tag/HEAD. 

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

The git log command displays committed snapshots. While git status lets you inspect the working directory and the staging area, git log only operates on the committed history. The -p option gives a detailed view.

### Reset

To return to previous versions
```
git reset --hard HEAD^   (the last version)
                 HEAD^^  (the second to last)
                 HEAD~10 (the 10th to last)
                 <commitId>
```

Note that ~ is useful for making relative references to the parent of a commit. For example, 3157e~1 refers to the commit before 3157e, and HEAD~3 is the great-grandparent of the current commit.

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

To create a new brach and navigate to it
```
git branch <branchname>
git checkout <branchname>
```
or type
```
git checkout -b <branchname>
```
to combine the two. Now changes are tracked in the new branch and master won't be affected.

Other commands related to branching are
* ```git branch``` checks the current branch and lists all
* ```git merge <branchname>``` merges the branch to the current branch
* ```git branch -d <branchname>``` deletes the branch
* ```git merge --no-ff -m "<note>" <branchname>``` does not delete the branch when merging

Note the easist merging mode is fast-forward: it simply moves the current branch tip to the target branch tip. 

When conflicts occur when you merge two branches, mannually resolve them by checking the repo's status and the branch merge graph by
```
git log --graph --pretty=oneline --abbrev-commit
```

When adding a new feature to your program, create a new branch for it so that code in the Master branch is not tourched. If the new feature is unfortunately discarded by the program manager, use
```
git branch -D <branchname>
```
to delete the unmerged branch.

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
git tag <tagname>
```
gives a new tag to the latest commit;
```
git tag <tagname> <commitID>
```
gives a tag to a specified commit;
```
git show <tagname>
```
returns details of a commit with a corresponding tag name;
```
git tag -a/s <tagname> -m "<notes>"
```
attaches the notes;
```
git tag -d <tagname>
```
deletes the local tag;
```
git push origin <tagname>
```
or
```
git push origin --tags
```
pushes the commit with the tag name. TO delete a remote repo, use
```
git tag -d <tagname>
```
to delete locally and then
```
git push origin :refs/tags/<tagname>
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
