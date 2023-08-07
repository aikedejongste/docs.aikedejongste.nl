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
