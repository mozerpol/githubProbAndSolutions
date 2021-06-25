## Repo description
I'll post here all (and especially the most popular) problems, that I have when I'm using the github.
------------

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

### 2. "*error: failed to push some refs to*" 
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
### 3. Downloading the latest version of commit
Copy it to the terminal
1. `git reset --hard HEAD`
2. `git clean -xffd`
3. `git pull`

### 4. Deleting the latest commit
If you want do this, put this command into terminal: `git reset --soft HEAD~1`

**It's very important!** After this you must add new changes, commit them and push with force flag. Without this you'll have a problem from *problem number 2*.
