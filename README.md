# Repo description
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
    1. -m
	2. -a
	3. -amend
12. [How to name commits](#12)
13. [Why can't I see my commits](#13)
14. [git after tab does not have prompts](#14)
15. [How to change default editor for commits](#15)
16. [Commit template](#16)
17. [What is a pull request?](#17)
18. [Updates were rejected because the remote contains work that you do not have
locally](#18) 
19. [](#19)

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
**Short desc:** When I try to upload updates to the repository after a successful
commit. <br/>
**Possible cause:** Changing the same repository in two different places 
(e.g. on the computer and via github.com) without subsequent synchronization. 
After that an attempt to upload a latest commit from computer to the github.

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

Similar problem on the stack overflow [date of access: 09.12.2020]: <br/>
https://stackoverflow.com/questions/24357108/git-updates-were-rejected-because-the-remote-contains-work-that-you-do-not-have 
### 3. Downloading the latest version of commit <a name="3"></a> [UP↑](#tof)
Copy it to the terminal
1. `git reset --hard HEAD`
2. `git clean -xffd`
3. `git pull`

### 4. Deleting the latest commit <a name="4"></a> [UP↑](#tof)
If you want do this, put this command into terminal: `git reset --soft HEAD~1`

**It's very important!** After this you must add new changes, commit them and 
push with force flag. Without this you'll have a problem from *problem number 2*.

### 5. Merging two git repo into one <a name="5"></a> [UP↑](#tof)
Based on
[this](https://saintgimp.org/2013/01/22/merging-two-git-repositories-into-one-repository-without-losing-file-history/) 
article (date of access: 24.06.2021).

Very important info. You will keep all history of commits, but unfortunately 
you'll lose the number of daily contributions from the table on your github 
profile page, I mean: <br/>
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
But very important info, you must be careful, you can't have two the same files 
in old and new repo like "README.md", in this case will be an error.
5. `git merge old_a/main --allow-unrelated-histories` <br/>
Why we must add
[--allow-unrelated-histories](#https://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories-on-rebase)? <br/>
After execution this instruction (in my case) will open nano editor to add 
comment, you can just close this without adding sth.
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
```shell
git branch myNewBranch        # Create local branch named "myNewBranch"
```

A remote branch is a branch on a remote location (in most cases *origin*). <br/>
You can push the newly created local branch *myNewBranch* to *origin*.
Now other users can track it.
```shell
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
```shell
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
```shell
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
There is a several ways, the best for me: <br/>
1. Default history, with no arguments: `git log`. Each commit is with SHA-1 
checksum, the author’s name and email, the date written, and the commit message.
2. Some abbreviated stats for each commit, such as how many files were changed,
number of deletions and insertions: `git log --stat`
3. Commits in one line with useful info: <br/>
`git log --pretty=format:"%h - %an, %ar : %s"`, specifiers: <br/>

| Specifier  | Description of output |
|:--:|:--:|
| %H | Commit hash |
| %h | Abbreviated commit hash |
| %T | Tree hash |
| %t | Abbreviated tree hash |
| %P | Parent hashes |
| %p |Abbreviated parent hashes |
| %an | Author name |
| %ae | Author email |
| %ad | Author date (format respects the --date=option) |
| %ar | Author date, relative |
| %cn | Committer name |
| %ce | Committer email |
| %cd | Committer date |
| %cr | Committer date, relative |
| %s | Subject |

But be careful, between *format:* and *"* there is no space. 

4. Add a little ASCII graph showing branch and merge history: <br/>
`git log --pretty=format:"%h - %an, %ar : %s" --graph`

5. Limiting log output: `git log --since=2.weeks`

Nice link about
[this](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History).

### 10. How to delete branch <a name="10"></a> [UP↑](#tof)
`git branch -d other_branch`

### 11. The meaning of the commit options <a name="11"></a> [UP↑](#tof)
The git `commit` command will save all staged changes, along with a brief 
description from the user, in a *commit* to the local repository.

#### -m
It's required message to commit. If the `-m` is not included with the `git commit`
command, you will be prompted to add a message in your default text editor.

#### -a
This option automatically stages all modified files to be committed. If new 
files are added the `-a` option will not stage those new files. Only files that 
the Git repository is aware of will be committed.

#### -amend
The `--amend` option allows you to change your last commit. Example: <br/>
`git commit --amend -m "An updated commit message"` and after this we have: <br/>
```shell
mozerpol@mozerpol-pc:~/Documents/github/githubProbAndSolutions$ git log --pretty=format:"%h, %s"

2c3cde4, an updated commit message
3b1c694, Desc printing out history
afac21f, Desc merge
60c1a42, Add about remote and local branches
8ee6914, Add a problem with git prompts
ac678ef, Add problem with project owner
3b28d87, Add small changes
4c43ff3, Add how to display the list of branches
73cc87b, Add table of contents
bb70703, Desc how to merging two git repo into one
```

### 12. How to name commits <a name="12"></a> [UP↑](#tof)
The seven rules of a great Git commit message: <br/>
1. begin the commit message with a single short (less than 50 character) line 
summarizing the change. The first line in a commit message is treated as the 
commit title. Sometimes a single line is fine (not every commit requires both 
a subject and a body.), especially when the change is so simple that no further 
context is necessary. Example of short (one line) commit: <br/>
```
Fix typo in introduction to user guide
```
Example of commit which need a bit of explanation and context: <br/>
```
Derezz the master control program

MCP turned out to be evil and had become intent on world domination.
This commit throws Tron's disc into MCP (causing its deresolution)
and turns it back into a chess game.
```
Commit messages with bodies are not so easy to write with the `-m` option. You’re
better off writing the message in a proper text editor.

2. Limit the subject line to 50 characters. <br/>
*Tip: If you’re having a hard time summarizing, you might be committing too
many changes at once.* It's make sense :D <br/>
So shoot for 50 characters, but consider 72 the hard limit.

3. Capitalize the subject line. <br/>
This is as simple as it sounds. Begin all subject lines with a capital letter.

4. Do not end the subject line with a period or dot. <br/>
Wrong: *Open the pod bay doors.* <br/>
Correct: *Open the pod bay doors*

5. Use the imperative mood in the subject line. <br/>
Imperative mood means: <br/>
- Clean your room
- Close the door
- Take out the trash

6. Wrap the body at 72 characters. <br/>
The recommendation is to do this at 72 characters, so that Git has plenty of 
room to indent text while still keeping everything under 80 characters overall.

7. Use the body to explain what and why vs. how. <br/>
[This](https://github.com/bitcoin/bitcoin/commit/eb0b56b19017ab5c16c745e6da39c53126924ed6) 
commit from Bitcoin Core is a great example of explaining what changed and 
why: <br/>
```shell
commit eb0b56b19017ab5c16c745e6da39c53126924ed6
Author: Pieter Wuille <pieter.wuille@gmail.com>
Date:   Fri Aug 1 22:57:55 2014 +0200

   Simplify serialize.h's exception handling

   Remove the 'state' and 'exceptmask' from serialize.h's stream
   implementations, as well as related methods.

   As exceptmask always included 'failbit', and setstate was always
   called with bits = failbit, all it did was immediately raise an
   exception. Get rid of those variables, and replace the setstate
   with direct exception throwing (which also removes some dead
   code).

   As a result, good() is never reached after a failure (there are
   only 2 calls, one of which is in tests), and can just be replaced
   by !eof().

   fail(), clear(n) and exceptions() are just never called. Delete
   them.
```
Of course in most cases, you can leave out details about how a change has been
made. 

Based on: https://chris.beams.io/posts/git-commit/

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

### 15. How to change default editor for commits <a name="15"></a> [UP↑](#tof)
Open file *$HOME/.gitconfig* and add line:
```
[core]
	editor = nvim
```

### 16. How to change commit template <a name="16"></a> [UP↑](#tof)
Commit template is prompt message when you commit. To change this, just create
file *.gitmessage.txt* inside your home dir. After this open file 
*$HOME/.gitconfig* and add line:
```
[commit]                                                                        
	template = ~/.gitmessage.txt    
```

To *.gitmessage.txt* you can add whatever, but I added: 
```shell


# Title: Summary, start upper case, don't end with a period or dot No more than
# 50 chars. Use the imperative mood as "Clean your room" or "Close the door".

# Blank line between title and body.

# Body: Explain *what* and *why* (not *how*). Include task ID (Jira issue).
# Wrap at 72 chars.


# At the end: Include Co-authored-by for all contributors. 
# Include at least one empty line before it. Format: 
# Co-authored-by: name <user@users.noreply.github.com>
#
# 1. Limit the subject line to 50 characters
# 2. Separate subject from body with a blank line
# 3. Capitalize the subject line
# 4. Do not end the subject line with a period
# 5. Use the imperative mood in the subject line
# 6. Wrap the body at 72 characters
# 7. Use the body to explain what and why vs. how
```

Notice that the first two lines are empty, it's for convinence. <br/>
How it looks: <br/>
![image](https://user-images.githubusercontent.com/43972902/127651846-75e2ef21-32a0-4e33-af01-a50a79e5be7c.png)

### 17. What is a pull request? <a name="17"></a> [UP↑](#tof)
When you open a pull request, you’re proposing your changes and requesting that
someone review and pull in your contribution and merge them into their branch. 
Pull requests show *diffs* (*differences*), of the content from both branches. 

Why is a git *pull request* not called a *push request*? <br/>
When you send a *pull request*, you're asking (requesting) the official repo
owner to pull some changes from your own repo. Hence "pull request". <br/>
By default a safety net is set so no one can push to your repo. You can set
others as a collaborator, then they can push. <br/>
Question on
[stackoverflow](https://stackoverflow.com/questions/21657430/why-is-a-git-pull-request-not-called-a-push-request). 

### 18. Updates were rejected because the remote contains work that you do not have locally <a name="18"></a> [UP↑](#tof)

The error occurs when for example I changed any file on the github website, push
this changes and next I come back to my local repo and without synchro this changes
I want push a new commit. 

Error log: <br/>
```shell
mozerpol@mozerpol-pc:/learningRISC-V/implementation$ git push -u origin main 

Username for 'https://github.com': mozerpol
Password for 'https://mozerpol@github.com': 
To https://github.com/mozerpol/learningRISC-V
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'https://github.com/mozerpol/learningRISC-V'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

What to do:
1. Very important line from my log is fifth line: <br/>
`! [rejected]        main -> main (fetch first)` <br/>
2. `git pull <remote> from:where`, <br/> in my case:
```shell
mozerpol@mozerpol-pc: /learningRISC-V/implementation$ git pull origin main:main 

remote: Enumerating objects: 8, done.
remote: Counting objects: 100% (8/8), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 6 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), done.
From https://github.com/mozerpol/learningRISC-V
 ! [rejected]        main       -> main  (non-fast-forward)
   40f9349..aee651b  main       -> origin/main
```
3. Git status: 
```shell
mozerpol@mozerpol-pc: /learningRISC-V/implementation$ git status 

On branch main
Your branch and 'origin/main' have diverged,
and have 3 and 2 different commits each, respectively.
  (use "git pull" to merge the remote branch into yours)

nothing to commit, working tree clean
```
4. Pull changes:
```shell
mozerpol@mozerpol-pc: /learningRISC-V/implementation$ git pull origin main 

From https://github.com/mozerpol/learningRISC-V
 * branch            main       -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 README.md | 17 +++++++++--------
 1 file changed, 9 insertions(+), 8 deletions(-)
```

5. Push changes: 
```shell
mozerpol@mozerpol-pc: /learningRISC-V/implementation$ git push -u origin main 

Username for 'https://github.com': mozerpol
Password for 'https://mozerpol@github.com': 
Enumerating objects: 36, done.
Counting objects: 100% (29/29), done.
Delta compression using up to 4 threads
Compressing objects: 100% (20/20), done.
Writing objects: 100% (20/20), 3.14 KiB | 1.57 MiB/s, done.
Total 20 (delta 6), reused 0 (delta 0)
remote: Resolving deltas: 100% (6/6), completed with 4 local objects.
To https://github.com/mozerpol/learningRISC-V
   aee651b..2fbf3db  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

This problem described on 
[StackOverflow](https://stackoverflow.com/questions/24357108/git-updates-were-rejected-because-the-remote-contains-work-that-you-do-not-have). 
