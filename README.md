# vifmpane

Currently when you open vifm with `vifm .` the current path will be opened onto
the left pane, despite on which pane you would like to open it.

This script in intended to fix this behaviour and allows to choose on which
pane to open current path on enter, and into which directory from which of the
panes to cd at the exit.

This script supports all types of vifm tabs: global and pane-wise (set with
`set tabscope=pane`), and [vifmimg](https://github.com/cirala/vifmimg) script
(if installed) which adds image preview with
[Überzug](https://github.com/seebye/ueberzug) into vifm.

## Installation

You may use your shell plugin manager.  For example, to install with
[zsh-snap](https://github.com/marlonrichert/zsh-snap) (zsh plugin manager)
`vifmpane` as well as `vifmimg` add next lines into your `.zshrc` file:

```
znap install cirala/vifmimg
znap install anuvyklack/vifmpane
```

Or clone this repo and symlink `vifmpane` file into the directory that contains in
the `$PATH` variable: 

```
git clone --depth 1 https://github.com/anuvyklack/vifmpane.git
cd vifmpane
ln -s $PWD/vifmpane $HOME/.local/bin/vifmpane
```

## Configuration

The behaviour of this script is configured with global variable `VIFM_PANE_STRATEGY`.
There are three strategies to choose on which vifm pane open `$PWD` and into
which directory navigate on exit.  Default value is `last`.

* `left` or `first` (both values are valid)\
  Sync shell with the vifm **left** pane.  On launch vifm the current path
  `$PWD` will be opened onto **left** pane. On exit cd to the directory on the
  vifm **left** pane.
* `right` or `second` (both values are valid)\
  Sync shell with the vifm **right** pane. When launch vifm the current path
  will be opened onto **right** pane. On exit cd to the directory on the vifm
  **rigth** pane.
* `last` (default)\
  On launch vifm the current path will be opened onto the vifm **last** active
  pane from the previous time you leave vifm. On exit cd into the directory
  from the **last** active pane.

## Usage

In bash/zsh to navigate into selected directory on exit run:

```
source vifmpane
```

it will execute the script in the current process but not in a subshell.

### zsh

To open vifm in current directory with `Ctrl + o` keybinding add next lines
into your `.zshrc` file.

```zsh
vifmcd() {
  zle .reset-prompt
  BUFFER=" source vifmpane"
  zle accept-line
}
zle -N vifmcd

# <C-o> - Launch vifm and cd to the last directory after closing it.
bindkey '^o' vifmcd
```

## Prerequisites

* [jq](https://github.com/stedolan/jq) — vifm stores its data between
  sessions in JSON format.  To install on Ubuntu/Debian execute:

  ```
  sudo apt-get install jq
  ```

* [vifmimg](https://github.com/cirala/vifmimg) — (optional) for image preview.
