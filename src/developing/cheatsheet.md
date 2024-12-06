# Cheatsheet

This contains the most important information to get you up and running and productive.

## Git

Open the command line in the folder where you downloaded DSAV-Dodeka.github.io / open the terminal in VS Code (Ctrl+`)

### General workflow

```
# go to the main branch
git checkout main
# update the repository
git pull
# go to a new branch (replace branchname with your desired name, no spaces or capital letters allowed)
git switch -c "branchname"
# add all edited files to future commit
git add -A
# commit the changes (change the description to something useful)
git commit -m "commit description"
# upload changes to github.com (replace 'branchname' with what you used earlier)
# in case you already pushed this branch before, you can just do git push
git push --set-upstream origin branchname
```

### Status

See the current status (shows what branch you are on)

```
git status
```

### Go to a branch

If you want to go to a particular branch, say 'branch-xyz', do:

```
git checkout branch-xyz
```

### Update a branch

If you want to update your current branch with changes on github.com:

```
git pull
```

If you have changes locally, this might not work.

### Delete all local changes (BE CAREFUL)

If you did some stuff you don't know how to revert, but also don't care to save it, do (be careful!):

```
git reset --hard
```

## Frontend

Open the command line in the folder where you downloaded DSAV-Dodeka.github.io / open the terminal in VS Code (Ctrl+`)

### Run the website locally

```
npm run dev
```

The website is now available in your browser at http://localhost:3000.

### Update dependencies

```
npm install
```