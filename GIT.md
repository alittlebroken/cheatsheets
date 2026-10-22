# GIT Cheatsheet

## Initialise git 

- Run the following to setup a project folder as a git repo
```bash
cd <project_folder>
git init
```

## Config setting

- Setting username and email globally
```bash
git config --global user-name <your username>
git config --global user.email <your email>
```

## CLI

### Common commands

- Check the current status of your repo
```bash
git status
```

### Branches

- Create a new branch
```bash
git branch <branch_name>
```

- Switch to a new branch
```bash
git checkout <branch_name>
```
- Create a new branch and switch to it
```bash
git checkout -b <branch_name>

or use the following

git switch -c <branch_name>
```

### Committing changes

- List changes ready to be added to staging
```bash
git status
```

- Add all changes to staging
```bash
git add .
```

- Add one file to staging
```bash
git add <file>
```

- Commit the changes
```bash
git commit -m "Initial commit"
```

- Push the changes to the remote repo
```bash
git push
```

