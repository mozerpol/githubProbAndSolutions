## Repo description
I'll post here all (and especially the most popular) problems, that I have when I'm using the github.

### 1. Creating new repo and pushing first commit

1. Go to tab `Repositories`
2. Click `New` and next fill all informations
3. Click Add README.md and go to next. It'll create branch main in your repo
4. Create folder on your computer and go there
5. `git init` in terminal
6. In your repository (on github) you can find green button `Code`, click on it and from drop-down list copy adress your repo.
7. `git clone` and adress to yout repo
8. This will cause download the repository to your computer
9. Now you can `add .` and `commit` to your repo

### 2. When I try to upload updates to the repository after a successful commit

`
mozerpol@mozerpol-pc: git push -u origin main
`

Problem:

```shell
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
Solution:

`
mozerpol@mozerpol-pc:~/Documents/MATLAB/matlabOPTSEgit$ git push -u --force origin main
`

### 3. Downloading the latest version of commit 
Copy it to the terminal
1. `git reset --hard HEAD`
2. `git clean -xffd`
3. `git pull` 







