## Repo description
I'll post here all (and especially the most popular) problems, that I have when I'm using the github.

------------

### Table of contents <a name="tof"></a>
1. The easiest way to create new repo and pushing first commit
2. [*error: failed to push some refs to*](#2)
3. [Downloading the latest version of commit](#3)
4. [Deleting the latest commit](#4)
5. [Merging two git repo into one](#5)
6. [How to display the list of branches](#6)
7. [How to change a branch](#7)
8. [How to merge a branch](#8)
9. [How to print out the history of commits with branches](#9)
10. [The meaning of the commit options](#10)
    1. [-m](#10.1) 

### 1. The easiest way to create new repo and pushing first commit 
We have two ways.

#### First:
1. Go to tab `Repositories`
2. Click `New` and next fill all informations
3. Click Add README.md and go to next. It'll create branch main in your repo
4. Create folder on your computer and go there
5. In your repository (on github) you can find green button `Code`, click on it and from drop-down list copy adress your repo.
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

Similar problem on stackOverflow [09.12.2020]: https://stackoverflow.com/questions/24357108/git-updates-were-rejected-because-the-remote-contains-work-that-you-do-not-have 
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

Merge repos, but before find files, which have the same name e.g. README.md in *old_a* and *old_b* folder. It's not allowed, you must get rid of this conflict and commit changes.
1. Extract *old_a* and *old_b* to a shared folder <br/>
`mv old_a/* ./` <br/>
`mv old_b/* ./`
2. `git add .`
3. `git commit -m "Extract old_a and old_b to a shared folder"`

### 6. How to display the list of branches <a name="6"></a> [UP↑](#tof)
### 7. How to change a branch <a name="7"></a> [UP↑](#tof)
### 8. How to merge a branch <a name="8"></a> [UP↑](#tof)
### 9. How to print out the history of commits with branches <a name="9"></a> [UP↑](#tof)
### 10. The meaning of the commit options <a name="10"></a> [UP↑](#tof)
#### -m <a name="10.1"></a>
