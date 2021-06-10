# git-clone

Makes having multiple Git accounts more manageable via separate Personal Access Tokens, HTTPS
repositories, hierarchical .gitconfigs, and your OS's keychain.

## Installation

`git-clone` is a bash script, so putting it anywhere in your $PATH is all you really need to do! I
prefer a shorter name, so I also add

```
alias clone=git-clone
```

to my .profile (or `.zshrc` or `.bashrc` or...)

> ~~Note: I am working on getting `git-clone` added to [Homebrew](https://brew.sh/), but I think it needs just a bit more refinement~~
>
> I read through the Homebrew guidelines and I don't believe `git-clone` fits the bill yet; maybe
> someday!

## Setup
Anytime you call `git-clone` with a new username it will:
  - Prompt for a Git Personal Access Token for the username. See
    - [GitHub](https://github.com/settings/tokens)
    - [GitLab](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
    - [BitBucket](https://confluence.atlassian.com/bitbucketserver/personal-access-tokens-939515499.html)
  - If you already have another username saved, prompt for a 'default' username
  - Create/update its config file
    - Located in ~/.config/git_clone/config
  - Create/update ~/.gitconfig.personal*

> NOTE: Be sure to use the username associated with the token; while GitHub allows HTTPS connections
> using only the token, other git systems may not; `git-clone` uses the full name:token syntax to be
> universally compatible

### Project-specific .gitconfig files

Optionally, you can also use `git-clone` to manage your git user.name and user.email per project. To
do this, you'll want to arrange your projects hierarchically:

```
├── username1           # This folder name doesn't matter - I prefer using client names
│   ├── .gitconfig      # user.name and user.email specific to username1
│   ├── projectA
│   └── projectB
└── username2           # This folder name doesn't matter - I prefer using client names
    ├── .gitconfig       # user.name and user.email specific to username2
    ├── projectC
    └── projectD
```

While cloning new repositories, `git-clone` will add an **include** section to the project's
`./git/config` file, allowing you to use different email addresses (or names) for different
projects.

Here's a quick example with some command line wandering:

```
  $) cd ~/Projects/Personal
  # Use the default (I usually set my personal account as default)
  $) git-clone https://github.com/Flare576/git_clone
  $) git-clone Flare576/dotfiles # user/project syntax Only works with github
  # I now need to access a client project
  $) cd ~/Projects/Client1
  $) cat .gitconfig
     [user]
       name: Flare FiveSeventySix Esquire
       email: flare.fiveseventysix@client.com
  $) git-clone -u client1.login https://bitbucketserver.com/scm/projectname/teamsinspace.git
     # git clone output
  $) cat teamsinspace/.git/config
     ...
     [include]
       path=/Users/flare576/Projects/Client1/.gitconfig
```

## Normal Use
Once setup, you can call `git-clone` with either:

```
  $) git-clone Flare576/dotfiles # github project, using default account
  $) git-clone https://github.com/Flare576/git_clone
  $) git-clone -u client1.login https://bitbucketserver.com/scm/projectname/teamsinspace.git
```

`git-clone` will:
- Use the `-u[sername]` you provide or, if absent, the GIT_DEFAULT_USER value from the config
- Clone project (optionally clones specific branch and/or recursively - see options)
- Updates project's .git/config to have 'include.path=../../.gitconfig'." (see folder structure
    above) if present

## TODO:

- Allow long-name options
- Determine if/how to use linux OS keychains
  - Is it different for each OS? Are there systems which don't have this concept? How does git
      proper handle this?
- Add tests
