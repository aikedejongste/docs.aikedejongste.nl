---
layout: default
title: Neovim
parent: Editors
has_children: true
---

# Neovim

## Config file

Stored in `cat ~/.config/nvim/init.vim`

```text
call plug#begin()


Plug 'github/copilot.vim'
Plug 'morhetz/gruvbox'

let g:copilot_filetypes = {'markdown': v:true}
let g:copilot_filetypes = {'yaml': v:true}

call plug#end()
" You can revert the settings after the call like so:
"   filetype indent off   " Disable file-type-specific indentation
"   syntax off            " Disable syntax highlighting
colorscheme gruvbox
" set paste
```
