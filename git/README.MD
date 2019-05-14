## Git
Git is a distributed version-control system for tracking changes in source code during software development

* [Move files to a subdirectory](#moves-files-to-a-subdirectory)
* [Delete local commits](#delete-local-commits)
* [Oh shit, git](#oh-shit-git)

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
