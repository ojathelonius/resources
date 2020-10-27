## Git
Git is a distributed version-control system for tracking changes in source code during software development

* [Move files to a subdirectory](#moves-files-to-a-subdirectory)
* [Delete local commits](#delete-local-commits)
* [Oh shit, git](#oh-shit-git)
* [Assume files untracked](#assume-files-untracked)
* [Clean local repository](#clean-local-repository)
* [Verify unpushed commits](#verify-unpushed-commits)
* [Delete older commit](#delete-older-commit)

### Move files to a subdirectory
Use git commands to move files around so that it properly keeps track of their history :

```bash
mkdir subdir
```

The `-k` option will skip move/rename errors due to moving untracked files or trying to move the subdir folder in itself.
```bash
git mv -k * subdir/
```

Be aware that untracked files will not be moved :
* generated files might not have to be moved anyway (node_modules, javagen...)
* config files have to be moved manually or with a regular `mv`

### Delete local commits
Use the following commands to delete local commits :

```bash
# Cancel last commit
git reset --hard HEAD^

# Cancel last X commits
git reset --hard HEAD~X
```

### Oh shit, git
List of useful git commands to fix common mistakes :

https://ohshitgit.com/

### Assume files untracked

Configuration file changes might not have to be included in the remote repo, while they can still be modified locally AND have a default version in the remote repository. Ignoring the file alone **does not work** since it's already tracked by git.

For this purpose, add the config file to `gitignore` and use :

```bash
git update-index --assume-unchanged config.ini
```

### Clean local repository

Update the local repo with :

```bash
git fetch --all
```

Remove local **branches** that do not exist anymore on the remote repo :

```bash
git remote prune origin && git checkout develop && git branch -vv | awk '/: gone]/{print $1}' | xargs git branch -d
```

Remove local **tags** that do not exist anymore on the remote repo :

```bash
git fetch --prune origin "+refs/tags/*:refs/tags/*"
```

### Verify unpushed commits

The following command will list all commits ahead of the remote branch.
```
git log @{push}..
```

### Delete older commit

```
git rebase -p --onto COMMIT_SHA^ COMMIT_SHA
```

Force pushing could be required if already pushed.
