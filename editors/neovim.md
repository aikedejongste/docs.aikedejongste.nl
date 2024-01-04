---
layout: default
title: Neovim
parent: Editors
has_children: false
---

# Neovim

## Config file

Stored in `cat ~/.config/nvim/init.vim`

```text
call plug#begin()


Plug 'github/copilot.vim'
Plug 'morhetz/gruvbox'
Plug 'pearofducks/ansible-vim'
Plug 'yaegassy/coc-ansible', {'do': 'yarn install --frozen-lockfile'}


let g:copilot_filetypes = {'markdown': v:true}
let g:copilot_filetypes = {'yaml': v:true}

call plug#end()
" You can revert the settings after the call like so:
"   filetype indent off   " Disable file-type-specific indentation
"   syntax off            " Disable syntax highlighting
colorscheme gruvbox
" set paste
```

## Plugin manager

[Vim-Plug](https://github.com/junegunn/vim-plug)

Install with:

```bash
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

## Plugins I like

- <https://github.com/pearofducks/ansible-vim>

- <https://github.com/yaegassy/coc-ansible>

## XML lint

`:%!xmllint --format %`

## Format json

`:%!python -m json.tool`

## Reformat text to line length

First, set the text width to 100 by using the following command in Normal mode:

`:set textwidth=100`

After selecting the text in Visual mode, simply press gq, and Vim will reformat
the selected text to the specified width.

Or in Normal mode, you can use gq followed by a motion command to reformat a
specific part of the text. For example, to reformat the current paragraph, you
would use `gqap`.

## Delete trailing whitespaces

`:%s/\s\+$//e`

## Delete everything after space on the same line

`:%s/\(\S\+\s\).*/\1`

## Slash at end of every line

`:%s/$/ \\/`

## When you forgot to use sudo

`:w !sudo tee %`
