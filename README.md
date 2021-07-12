## Repo description
I'll post here all (and especially the most popular) problems or tips, that I 
have when I'm using the git.

------------

### Table of contents <a name="tof"></a>
1. The easiest way to create new repo and pushing first commit
2. [*error: failed to push some refs to*](#2)
3. [Downloading the latest version of commit](#3)
4. [Deleting the latest commit](#4)
5. [Merging two git repo into one](#5)
6. [How to display the list of branches](#6)
	1. Differences between local branch and remote branch.
	2. Differences between remote tracking branch and local tracking branch.
7. [How to change a branch](#7)
8. [How to merge a branch](#8)
9. [How to print out the history of commits with branches](#9)
10. [How to delete branch](#10)
11. [The meaning of the commit options](#11)
    1. [-m](#11.1) 
12. [How to name commits](#12)
13. [Why can't I see my commits](#13)
14. [git after tab does not have prompts](#14)

### 1. The easiest way to create new repo and pushing first commit 
We have two ways.

#### First:
1. Go to tab `Repositories`
2. Click `New` and next fill all informations
3. Click Add README.md and go to next. It'll create branch main in your repo
4. Create folder on your computer and go there
5. In your repository (on github) you can find green button `Code`, click on it
and from drop-down list copy adress your repo.
6. `git clone` and adress to your repo

#### Second:
1. `git init` in terminal
2. Add something inside like README
3. `git add .`
4. `git commit -m "Comment"`
5. `git clone` and adress to yout repo
6. Go to github
7. Go to tab `Repositories`
8. Click `New` and next fill all informations
9. Click on green button `Create repository`
10. Go to terminal
11. `git remote add origin https://github.com/mozerpol/_name_of_your_repo_from_github_`
12. `git branch -M main`
13. `git push -u origin main`

### 2. "*error: failed to push some refs to*" <a name="2"></a> [UP↑](#tof)
**Short desc:** When I try to upload updates to the repository after a successful commit. <br/>
**Possible cause:** Changing the same repository in two different places (e.g. on the computer and via github.com) without subsequent synchronization. After that an attempt to upload a latest commit from computer to the github.

**Problem:**
```shell
`mozerpol@mozerpol-pc: git push -u origin main`

Username for 'https://github.com': mozerpol
Password for 'https://mozerpol@github.com': 
To https://github.com/mozerpol/OPTSE.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/mozerpol/OPTSE.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

**First solution:**
1. `mozerpol@mozerpol-pc:~/Documents/MATLAB/matlabOPTSEgit$ git push -u --force origin main`

**Second solution (check if it's correct):**
1. `git pull origin main:dev`
2. `git push -u origin main`

Similar problem on the stack overflow [09.12.2020]: https://stackoverflow.com/questions/24357108/git-updates-were-rejected-because-the-remote-contains-work-that-you-do-not-have 
### 3. Downloading the latest version of commit <a name="2"></a> [UP↑](#tof)
Copy it to the terminal
1. `git reset --hard HEAD`
2. `git clean -xffd`
3. `git pull`

### 4. Deleting the latest commit <a name="4"></a> [UP↑](#tof)
If you want do this, put this command into terminal: `git reset --soft HEAD~1`

**It's very important!** After this you must add new changes, commit them and push with force flag. Without this you'll have a problem from *problem number 2*.

### 5. Merging two git repo into one <a name="5"></a> [UP↑](#tof)
Based on [this](#https://saintgimp.org/2013/01/22/merging-two-git-repositories-into-one-repository-without-losing-file-history/) article (date of access: 24.06.2021).

Very important info. You will keep all history of commits, but unfortunately you'll lose the number of daily contributions from the table on your github profile page, I mean: <br/>
![obraz](https://user-images.githubusercontent.com/43972902/123415620-d7211b00-d5b5-11eb-9b38-f15687b23ebe.png)

Step by step:
1. Create new repo (you can use way from first step in this repo).
2. Create inside new file (later we'll delete it). <br/>
`touch deleteme.txt` <br/>
3. Commit this changes <br/>
`git add .` <br/>
`git commit -m "Initial dummy commit"`
4. Add a remote for and fetch the first old repo. <br/>
`git remote add -f old_a <link to the first project>`, example: <br/>
`git remote add -f old_a https://github.com/mozerpol/learningVerilog.git` <br/>
But very important info, you must be careful, you can't have two the same files in old and new repo like "README.md", in this case will be an error.
5. `git merge old_a/main --allow-unrelated-histories` <br/>
Why we must add [--allow-unrelated-histories](#https://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories-on-rebase)? <br/>
After execution this instruction (in my case) will open nano editor to add comment, you can just close this without adding sth.
6. Delete first unnecessary commit and commit changes. <br/>
`rm deleteme.txt` <br/>
`git commit -m "Clean up initial file"`
7. Create a new folder where you want to move the old repository <br/>
`mkdir old_a`
8. Move everything there except the folder you are moving to and the *.git* folder <br/>
`mv !(.git|old_a) ./old_a/`
9. `git add .`
10. `git commit -m "Move old_a files into subdir"`

Do the same thing for old_b

1. `git remote add -f old_a <link to the first project>`
2. `git merge old_a/main --allow-unrelated-histories`
3. `mkdir old_b`
4. `mv !(old_a|old_b|.git) ./old_b/`
5. `git add .`
6. `git commit -m "Move old_b files into subdir"`

Merge repos, but before find files, which have the same name e.g. README.md in 
*old_a* and *old_b* folder. It's not allowed, you must get rid of this conflict 
and commit changes.
1. Extract *old_a* and *old_b* to a shared folder <br/>
`mv old_a/* ./` <br/>
`mv old_b/* ./`
2. `git add .`
3. `git commit -m "Extract old_a and old_b to a shared folder"`

### 6. How to display the list of branches <a name="6"></a> [UP↑](#tof)

See what branch you're on: `git status` <br/>
To see local branches: `git branch` <br/>
To see remote branches: `git branch -r` <br/>
To see all local and remote branches: `git branch -a` <br/>

#### Differences between local branch and remote branch.
A local branch is a branch that only you (the local user) can see. It exists 
only on your local machine.
```
git branch myNewBranch        # Create local branch named "myNewBranch"
```

A remote branch is a branch on a remote location (in most cases *origin*). <br/>
You can push the newly created local branch *myNewBranch* to *origin*.
Now other users can track it.
```
git push -u origin myNewBranch   # Pushes your newly created local branch "myNewBranch"
                                 # to the remote "origin".
                                 # So now a new branch named "myNewBranch" is
                                 # created on the remote machine named "origin"
```

#### Differences between remote tracking branch and local tracking branch.
A remote tracking branch is a local copy of a remote branch. When *myNewBranch*
is pushed to *origin* using the command above, a remote tracking branch named 
*origin/myNewBranch* is created on your machine. This remote tracking branch 
tracks the remote branch *myNewBranch* on *origin*. You can update your remote 
tracking branch to be in sync with the remote branch using `git fetch` or 
`git pull`.
```
git pull origin myNewBranch      # Pulls new commits from branch "myNewBranch" 
                                 # on remote "origin" into remote tracking
                                 # branch on your machine "origin/myNewBranch".
                                 # Here "origin/myNewBranch" is your copy of
                                 # "myNewBranch" on "origin"
```

A local tracking branch is a local branch that is tracking another branch. 
This is so that you can push/pull commits to/from the other branch. Local
tracking branches in most cases track a remote tracking branch. When you push a
local branch to *origin* using the `git push` command with a `-u` option (as shown 
above), you set up the local branch *myNewBranch* to track the remote tracking 
branch *origin/myNewBranch*. This is needed to use `git push` and `git pull` 
without specifying an upstream to push to or pull from.
```
git checkout myNewBranch      # Switch to myNewBranch
git pull                      # Updates remote tracking branch "origin/myNewBranch"
                              # to be in sync with the remote branch "myNewBranch"
                              # on "origin".
                              # Pulls these new commits from "origin/myNewBranch"
                              # to local branch "myNewBranch which you just switched to.
```

### 7. How to change a branch <a name="7"></a> [UP↑](#tof)
`git checkout <existing_branch>`

If the destination branch does not exist: <br/>
`git checkout -b <new_branch>`

### 8. How to merge a branch <a name="8"></a> [UP↑](#tof)
So... If we want merge *other_branch* to *master* we must do: <br/>
1. Checkout on *master* branch, by `git checkout master`
2. Then `git merge other_branch`

Now *master* and *other_branch* are the same, so we can delete *other_branch*: <br/>
`git branch -d other_branch`

### 9. How to print out the history of commits with branches <a name="9"></a> [UP↑](#tof)
### 1o. How to delete branch <a name="10"></a> [UP↑](#tof)
### 11. The meaning of the commit options <a name="11"></a> [UP↑](#tof)
#### -m <a name="11.1"></a>
### 12. How to name commits <a name="12"></a> [UP↑](#tof)
### 13. Why can't I see my commits <a name="13"></a> [UP↑](#tof)
Can be a several possibilities, in my case it was fact that email address used 
for the commits is associated with different github account. <br/>
To change this just change project email to your email from github. To do this: <br/>
1. Check the project owner (email): <br/>
`git config --global user.email` <br/>
If is different, then:
2. `git config --global user.email "YOUR_GITHUB_EMAIL_ADDRESS"`  

### 14. "*git after tab does not have prompts*"  <a name="14"></a> [UP↑](#tof)
After *git* installation I had a problem with git propter. Perhaps problem was
with *bash*. I repaired it just: `sudo apt install git-core bash-completion`. <br/>
Problem on described on StackOverflow: <br/>
https://stackoverflow.com/questions/12399002/how-to-configure-git-bash-command-line-completion



