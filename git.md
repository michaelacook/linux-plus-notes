# Git version control
- more detailed notes [here](https://github.com/michaelacook/fullstackjs-notes/blob/master/git-github/git-basics.md)
- source code management (scm)
- tracking changes in code
- can be distributed
- allows for collaboration
- allows versioning of files
- was created by Linus Torvalds for use by the kernel developers and is native to Linux
- clone a remote repository with `git clone [repo]`
- locally clone a repository with `git clone --local [repo location] .`
  - if a repository exists on your local machine and you want to clone it to another directory, or if you have a repo that you cloned from the web in one place and want to clone the local copy to another, use this option

## configuration
- can do a global or non-global configuration
- global config

```bash
git config --global user.name "name"
git config --global user.email "email"
git config --global core.editor "/usr/bin/vim"
```

- local config 

```
git config --local user.name "name"
git config --local user.email "email"
git config --local core.editor "/usr/bin/vim"
```

- local config for a repo is located at `./.git/config` inside the `.git` directory in the repo
  - unless you specify a local configuration, the config for a repo will be the global config
- view configuration with `git config --list`
- view origin of configuration with `git config --list --show-origin`
  - used to distinguish between local and global configs 
  - local author may be different from the global author
  - local configs override global configs
- initialize empty git repo with `git init`
- specify files and folders for git to ignore in `.gitignore`

## git filesystem

```bash
.
├── branches
├── COMMIT_EDITMSG
├── config
├── description
├── HEAD
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── prepare-commit-msg.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── index
├── info
│   └── exclude
├── logs
│   ├── HEAD
│   └── refs
│       └── heads
│           └── master
├── objects
│   ├── c4
│   │   └── 985aa7ea64c3aa9ec646ca4610bfa0d3b660b4
│   ├── df
│   │   └── 2b8fc99e1c1d4dbc0a854d9f72157f1d6ea078
│   ├── e6
│   │   └── 9de29bb2d1d6434b8b29ae775ad8c2e48c5391
│   ├── info
│   └── pack
└── refs
    ├── heads
    │   └── master
    └── tags
```

- `HEAD` points to the current directory of the branch in `./.git/refs/heads`
- `config` where local repo configuration is stored
- `hooks` store scripts to run when a commit or merge is made
- `logs`
- `objects` actual store where objects are placed when you make a commit 
  - directories are first two characters of the commit ID, file name is the rest of the characters in the commit ID

## Branching
- used to separate code changes
- allow multiple users to work on the same code and then merge changes together
  - code is "checked out"
  - code is changed/updated and tested
  - code then merged back into the master branch
- see which branch you're in with `git status`
- switch into a new branch with `git checkout [branch name]`
  - add `-b` option to create the branch if it doesn't exist
- delete a branch with `git branch -d [branch name]`