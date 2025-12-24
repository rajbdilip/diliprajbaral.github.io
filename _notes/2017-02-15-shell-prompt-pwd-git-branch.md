---
title: "Customize Shell Prompt to Show Current Working Directory (PWD) and Current Git branch"
date: 2017-02-15
excerpt: "Add PWD and Git branch to your shell prompt so you can see where you are and what branch you’re on at a glance."
tags:
  - shell
  - prompt
  - git
---

```bash
# Add the following lines of code to ~/.bash_profile or ~/.profile

parse_git_branch() {
     git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(parse_git_branch)\[\033[00m\] $ "
```

This updates your prompt to show the current working directory and Git branch in every terminal (IDE or standalone).

![Example prompt showing PWD and Git branch](/assets/images/notes/bash-prompt.png)

This has been a life-saver, keeping me from constantly running `pwd` and `git branch`.
