This file mostly shows useful commands on git.

## Branches
```shell
# Create and checkout to the new branch
git checkout -b new_branch_name

# Delete branch
git branch -d new_branch
git branch -D new_branch # force deletion

# Only switch branches
git switch develop
```
## Commits
```shell
# Add every tracked file to the staging area and commit
git commit -am "title" -m "desc"

# Rewrite the last commit message.
# This command will open a text editor depending on your system.
git commit --amend
```

## Remotes
```shell
# Adding a remote repository
# Here origin is the name you want for a given remote repository
git remote add origin git@url

# Change remote link
git remote set-url origin git@another-url

# Remove remote repostiory
git remote rm origin

# List remote repositories
git remote -v
```

## Stash
```shell
# Save changes for later and clean the current file back to the HEAD
git stash

# Visualize the stash
git stash list
git stash show

# Apply the stashed changes on top of the current branch files
# This can cause conflicts
git stash apply

# Delete the stashed commits
git stash pop
```
