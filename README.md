# Learning Git

## Introduction

This repo contains the notes I made while learning the Git command-line tool from [Liao Xuefeng's blog](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000). Git is an advanced distributed version control tool that enables release, cooperation and evolution. Mastering Git really helps boost the productivity and maintain the software.

A Git command cheatsheet by Tower is also provided here for your reference.

## Commands

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

The Git global configuration file is stored in main/.gitconfig while the local repo configuration file resides in <repo>/.git/config. You could either change your configuration in a text editor or use
```
git config --global user.name "<YourName>"
git config --global user.email "<email@example.com>"
```

### Initialization

To initialize a Git repo, you could either initialize it locally by
```
git init
```
just note to use UTF-8 encoding without BOM under Windows. Or you could clone a remote repo by
```
git clone <protocol>
```
where the protocol could be either https or git. Currently, git is the fastest protocol supported but https is safer.

### Modification

After changing files, including adding, modifying and removing, 
```
git add <file1>
git add <file2>
...
git commit -m "<note>"
```
to commit the changes. If many files are changed, could use
```
git add --all
```
Remember, every time files are changed, git add and git commit.

To remove a file, first delete it in the repo then
```
git rm <filename>
```
and commit the change. Or directly type
```
git checkout -- <filename>
```

###  Status and Logging

To check the staus of the repo, type
```
git status
```
To see how a file is changed
```
git diff <file>
```
The logging of the repo is shown by
```
git log [--pretty=oneline]
git reflog
```

### Reset

To return to previous versions
```
git reset --hard HEAD^   (the last version)
                 HEAD^^  (the second to last)
                 HEAD~10 (the 10th to last)
                 <commitId>
```

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
git push -u origin master
```
where the -u parameter synchronizes the local repo and the remote repo and is only needed the first time you push the local to the remote repo; ever since now you can git push origin master after git commit.

### Branching

The main branch is called Master and there is actually a HEAD pointer that points to the current version of Master. Since HEAD is just a pointer, it could point to any other branch.

To create a new brach and navigate to it
```
git branch <branchname>
git checkout <branchname>
```
or type
```
git checkout -b <branchname>
```
to combine the two. Now changes are tracked in the new branch and Master won't be affected.

Other commands related to branching are
* ```git branch``` checks the current branch and lists all
* ```git merge <branchname>``` merges the branch to the current branch
* ```git branch -d <branchname>``` deletes the branch
* ```git merge --no-ff -m "<note>" <branchname>``` does not delete the branch when merging

When conflicts occur when you merge two branches, mannually resolve them by checking the repo's status and the branch merge graph by
```
git log --graph --pretty=oneline --abbrev-commit
```

When adding a new feature to your program, create a new branch for it so that code in the Master branch is not tourched. If the new feature is unfortunately discarded by the program manager, use
```
git branch -D <branchname>
```
to delete the unmerged branch.

### Bug

Stash is used to store the state of the git espcially when you need to add a new branch with work not pushed in the mean time. This makes bug fixes even easier.

To fix the bug, first navigate to the branch where bugs occur and create a new branch and then navigate to the previous branch to recover the Stash using
```
git stash list
git stash apply
git stash drop
```
or use 
```
git stash pop
```
to combine the two. If there are multiple stashes, 
```
git stath list
git stash apply stash@{...}
```

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

Before cooperation, ```git remote``` shows info about the remote repo and ```git remote -v``` gives details of the repo. When you want to push an entire branch, use
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
  
To contribute to an existing project, first fork the repo, then clone, add soemthing, push to your own repo and send pull request to contribute.

### gitignore

.gitignore tracks the file not to add to the repo. This file should include but not limited to system-generated cache, intermediate and executable files or ones containing sensitive info.

When Git reminds you of conflict,
```
git add -f <filename>
```
or
```
git check-ignore -v <filaname>
```
