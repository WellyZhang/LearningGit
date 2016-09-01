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
Init Git Repo
    git init
    ( note for encoding issues, choose UTF-8 )
    ( UTF-8 without BOM )
    
Adding
    git add <file>
    git add --all
    ...
    git commit -m "<note>"
    
Checking
    git status
    git diff <file>

After modification
    git add + git commit
    
Log
    git log
    or git log --pretty=oneline
    or git reflog
    
Reset
    git reset --hard HEAD^   ( the last version )
                     HEAD^^  ( the second to last )
                     HEAD~10 ( the 10th to last )
                     <commit id>
                     
Stage
    Stage is a temporay space for changes into files
    "git add" stores the changes instead of files into the Stage
    "git commit" pushes the changes in Stage to the branch
    git diff HEAD -- <filename> tracks the difference between the working directory and the branch
    
Unstage
    Case 1. git checkout -- <filename> discards changs in the working directory to a state of the nearest "git add" or "git commit"
    Case 2. if you git add, use git reset HEAD <filename> to unstage.
    
Deletion
    after rm <filename>
    git rm <filename>
    git commit -m "note" 
    or after rm <filename>
    git checkout -- <filename>
    
Distributed Git
    cd to .ssh
    ssh-keygen -t rsa -C "youremail@example.com"
    Key pressing Enter
    Log into the GitHub account setting web and copy the contents of .ssh/id_rsa.pub to ssh key
    
Remote Repo
    For a new repo in GitHub
    git remote add origin https://github.com/WellyZhang/LearningGit.git
    origin is the default name of the remote repo
    then git push -u origin master
    the -u parameter synchronize the local repo and the remote repo
    use it the first time for a new git remote repo
    since now you can git push origin master after git commit

Git Clone
    git clone <http/git>
    git is the fastest protocol supported

Branching
    Main branch is called Master
    HEAD points to the current version of Master
    but HEAD is just a pointer, it can point to any other branch
    git branch <branchname> creates a new branch
    git checkout <branchname> switch to another branch
    git checkout -b <branchname> combines the two commands
    now changes are tracked in the new branch and Master won't be affected
    git branch checks the current branch and lists all
    git merge <branchname> merges the branch to the current branch
    git branch -d <branchname> deletes the branch
    git merge --no-ff -m "note" <branchname> does not delete the branch when merging

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
    
    
    
    
    
    
    
    
