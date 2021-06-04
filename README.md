# hub-clone

Makes having multiple GitHub accounts more manageable via separate Personal Access Tokens, HTTPS
repositories, hierarchical .gitconfigs, and your OS's keychain.

## Installation

`hub-clone` is a bash script, so putting it anywhere in your $PATH is all you really need to do! I
prefer a shorter name, so I also add

```
alias clone=hub-clone
```

to my .profile (or `.zshrc` or `.bashrc` or...)

> Note: I am working on getting `hub-clone` added to [Homebrew](https://brew.sh/), but I think it
> needs just a bit more refinement

## Setup
Anytime you call `hub-clone` with a new username it will:
  - Prompt for a GitHub Personal Access Token for the username. See https://github.com/settings/tokens
  - If you have more than one username saved, prompt for a 'default' username
  - Create/update its config file
  - Create/update ~/.gitconfig.personal

Optionally, you can also use `hub-clone` to manage your git user.name and user.email per project. To
do this, you'll want to arrange your projects hierarchically:

```
├── username1           # This folder name doesn't matter - I prefer using client names
│   ├── .gitconfig      # user.name and user.email specific to username1
│   ├── projectA
│   └── projectB
└── username2           # This folder name doesn't matter - I prefer using client names
    ├── .gitconfig       # user.name and user.email specific to username1
    ├── projectC
    └── projectD
```

## Normal Use
Once setup, you can call `hub-clone` with either:

```
  $) hub-clone https://github.com/user_or_org/project_name
  $) hub-clone user_or_org/project_name
```

`hub-clone` will:
- Use the `-u[sername]` you provide or, if absent, the GIT_DEFAULT_USER value from the config
- Clone project (optionally clones specific branch and/or recursively)
- Updates project's .git/config to have 'include.path=../../.gitconfig'." (see folder structure
    above)

## Options:

> Note: currently only the short versions of these options work - allowing the long name is on my
TODO list

-u[sername]   The github username to use
-g[itconfig]  Modify ~/.gitconfig.personal
-d[efault]    Update default user
-b[branch]    Checks out a specific branch of the project before continuing
-r[ecurse]    Recursively checkout (for projects with submodules)

Config is stored in ${config}, and `hub-clone` will help manage your ~/.gitconfig.personal file as well.*

It is recommended that you structure your projects like
## TODO:

- Allow long-name options
- Determine if/how to use linux OS keychains
  - Is it different for each OS? Are there systems which don't have this concept? How does git
      proper handle this?
- Add tests
- Verify that username-specific .gitconfig exists in root before updating project's
- Change -d to --manage and allow setting default and deleting entries
