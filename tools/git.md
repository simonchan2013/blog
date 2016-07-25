### [(stackoverflow)Push a new local branch to a remote Git repository and track it too](http://stackoverflow.com/questions/2765421/push-a-new-local-branch-to-a-remote-git-repository-and-track-it-too)
```shell
git checkout -b <newBranchName>

# edit files, add and commit

git push -u origin <newBranchName>
```



### [(stackoverflow)How to get the git repository that I cloned before?](http://stackoverflow.com/questions/8051259/how-to-get-the-git-repository-that-i-cloned-before)
```shell
# get git repository in command line
git remote show origin
# just get url
git remote show origin | grep "Fetch URL"
```
