# zsh-utils

## TL;DR

```zsh
git clone https://github.com/quiram/zsh-utils.git
cd zsh-utils
./make-available
```

Enjoy zsh-utils :)

## Details

Port to zsh of [bash-utils](https://github.com/quiram/bash-utils) (still in progress).

Useful bits of zsh code, separated by categories. Each folder will have a number of scripts, each of them with comments
at the beginning explaining objective and usage. For a longer explanation check the following presentation:

[![Power up your development experience with bash scripts](http://img.youtube.com/vi/engxUm-Cji4/0.jpg)](https://youtu.be/engxUm-Cji4 "Power up your development experience with bash scripts")

Many of the commands support defaults in the form of environment variables, check them out!

The best way to make use of this repository is to place wherever you want to keep it and then run `make-available`. You
will need to reload your zsh defaults (typically `source $HOME/.zshrc`, depending on
your system) for changes to be effective in other open shell sessions.
