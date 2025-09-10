# git-clone

Makes having multiple Git accounts more manageable via separate Personal Access Tokens, HTTPS
repositories, hierarchical .gitconfigs, and GITHUB_TOKEN.

## Installation

`git-clone` is a bash script, so putting it anywhere in your $PATH is all you really need to do!

You can also do `brew install flare576/scripts/git-clone`

I prefer a shorter name, so I also add

```
alias clone=git-clone
```

to my .profile (or `.zshrc` or `.bashrc` or...)

## Normal Use

Once setup, you can call `git-clone` with Github shortnames like
[hub](https://github.com/github/hub) or any other git project with full HTTPS URL:

```bash
  $) git-clone Flare576/dotfiles       # Github only
  $) git-clone https://github.com/Flare576/git_clone
  $) git-clone -u client1.login https://bitbucketserver.com/scm/projectname/teamsinspace.git
```

`git-clone` will:
- Use the `-u[sername]` you provide or, if absent, the GIT_DEFAULT_USER value from the config
- Use the saved token (or run Setup) for that user
- Clone project (optionally clones specific -b[ranch] and/or -r[ecursively] - see options)
- Updates project's .git/config to have 'include.path=../../.gitconfig'." (see [Project-specific .gitconfig files](#project-specific-gitconfig-files)) if the project's parent directory has a .gitconfig file

## Setup

Anytime you call `git-clone` with a new username it will:
  - Prompt for a Git Personal Access Token for the username. See
    - [GitHub](https://github.com/settings/tokens)
    - [GitLab](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
    - [BitBucket](https://confluence.atlassian.com/bitbucketserver/personal-access-tokens-939515499.html)
  - If you already have another username saved, prompt for a 'default' username
      * If not, it'll write your token to `-t[oken_file]` as `GITHUB_TOKEN`
  - Create/update `git-clone` config file
    - Located in ~/.config/git_clone/config
  - Offer to Create/update ~/.gitconfig.personal

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
  # no .gitconfig file in this folder; it uses the ~/.gitconfig
  $) git-clone https://github.com/Flare576/git_clone
  $) git-clone Flare576/dotfiles
  # I now need to access a client project
  $) cd ~/Projects/Client1
  $) cat .gitconfig
     [user]
       email: flare.fiveseventysix@client.com
       name: Flare FiveSeventySix Esquire
  $) git-clone -u client1.login https://bitbucketserver.com/scm/projectname/teamsinspace.git
     # git clone output
  $) cat teamsinspace/.git/config
     ...
     [include]
       path=/Users/flare576/Projects/Client1/.gitconfig
```

## TODO:

- ~~Allow long-name options~~ After some research, this is a low priority
- Add tests
