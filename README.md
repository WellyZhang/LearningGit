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
git branch <branchname>h
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

Conflict resolving
    When conflicts occur when you merge two branches, mannually resolve them
    check by git status
    git log --graph --pretty=oneline --abbrev-commit
    shows the branch merging graph
    
Bug
    use git stash to store the state of the git especially when you need to add a new branch while you still have work not pushed
    then navigate to the branch where bugs occur and git a new branch to fix it
    after that navigate to the previous branch and recover the stash
    git stash list
    git stash apply and git stash drop
    or 
    git stash pop to combine the two
    if you have multiple stashes, git stath list and git stash apply stash@{...}
    
Feature
    add a new branch for each new feature
    use git branch -D <branchname> to delete if you have not git merge
    
Cooperation
    git remote shows info about the remote repo
    git remote -v gives details
    to push branch git push origin <branchname>
    Again origin is the name of the remote repo
    git clone by default only provides access to origin
    git checkout -b dev origin/dev opens the new branch
    when there is conflict, first git pull then 
    git checkout -b <branchname> origin/<branchname>
    git pull --set-upstrem <branchname> origin/<branchname>
    to establish connection of the local and remote repos
    resolve the conflict locally
    then git push
    
Tagging: alias of a commit id
    navigate to the branch
    git tag lists all the branch
    git tag <tagname> gives a new tag to the latest commit
    git tag <tagname> <commit id> gives a tag to a specified commit
    git show <tagname> returns details of a commit with a corresponding tag name
    git tag -a <tagname> -m "notes" attaches the notes
    if replace "-a" with "-s", git uses a encrypted tagging method
    git tag -d <tagname> deletes the local tag
    git push origin <tagname> pushes the commit with the tag name
    or git push origin --tags
    to delete remote tag
    git tag -d <tagname> locally
    then 
    git push origin :refs/tags/<tagname>
    
Contribute
    TO contribute to an existing project, first fork the repo, then clone, add soemthing and push
    pull request to contribute
    
GitIgnore
    .gitignore tracks the file extensions to not to add to the repo
    should include but not be limited to system-generated cache, intermediate and executable files or ones containing sensitive info
    if git reminds you of conflict, git add -f <filename>
    or git check-ignore -v <filaname>
    
    
    
    
    
    
    
    
