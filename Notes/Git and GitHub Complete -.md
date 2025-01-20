**Resources** 
[Git for Professionals Tutorial - Tools & Concepts for Mastering Version Control with Git - YouTube](https://www.youtube.com/watch?v=Uszj_k0DGsg)
[Git Tutorial for Beginners: Learn Git in 1 Hour - YouTube](https://www.youtube.com/watch?v=8JJ101D3knE)

**Commit Commands -** 
```git
git status    //view the changes done

git add .     //add all changed

git add <filename>   // add this changed file

git diff <filename>  //see the changes in the file

git add -p <filename>  //goes change by change(chunkwise) on every change and asks us to include this in the commit or not.
```

**Pull Requests -** 
Based on branches and not commits. 
Steps - 
1. Fork the repo
2. Clone the forked repo
3. make ur changes
4. use `git branch branch_name` to make a new branch and then `git checkout branch_name` to switch to that branch.
5. see the status, add the desired changes, normally since we are on the desired separate branch already.
6. make the commit `git commit -m "changes"`, then `git push --set-upstream origin branch_name` 
7. Now on the browser, github will ask if u wanna open a pull request now.
8. Now, click it and select which branch of the original repo we want it to be merged with, then add descriptions of the changes made, create the PR. DONE.

**Merge Conflicts -** 
happens when the changes are conflicting, like the same line changed in two different ways in 2 different commits, then git cant decide which is the one we want to keep, so needs human intervention.
also when a file was modified in one, but deleted in another before the modifications even happened in that first commit.
When we have a merge conflict, git will tell us and we can always abort as well, after a failed merge, just use `git merge --abort` or `git rebase --abort` in case of rebasing.
Simply clean the file to make the conflict go away. 
Use `git mergetool` to see the problems in an editor and then choose which to keep and which to not. 

**some .gitignore things -** 
If a file is already in the staging area, and then u try to add it to the .gitignore, it will get added but git wont stop tracking it. 
To make sure git really ignores it, remove it from the staging area, by doing `git rm --cached -r filename`  if no  --cached is added, the file is removed from the system as well.
-r is to allow for recursive removal when adding a whole directory to the .gitignore

**How to pull request from Source control-**
1. fork
2. clone the repo
3. make changes
4. Stage the changes
5. click on the three dots in the source control panel
6. select branch > create new branch, enter new branch name and commit
7. The button will turn to publish branch, publish it
8. Now on the browser, github will ask if u wanna open a pull request now.
Now, the tip is that, every now and then, checkout to main and sync the other PRs being merged in with ur local repo.
steps - 
1. Ctrl+Shift+P -> checkout main, then for once we need to add the original repo to our upstream(not the fork, the original).
2. `git remote ad upstream <original repo URL>`, this will add it so that we can fetch the new changes happening in the repo. 
3. Now, `git fetch upstream` to fetch all the new changes from there.
4. Then go to the source control 3 dots and pull > pull from upstream/main(updates our local repo), then sync changes(the sync just pushes the original repo changes to ur forked repo).