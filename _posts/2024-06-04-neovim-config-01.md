---
title: "Neovim Config 01"
author: "dog_egg"
date: 2024-06-04 15:30:00 -0400
categories: [Blogging, Dotfile]
tags: [neovim, plugins, config]
description: "Neovim Configuration Log"
toc: true # false to disable the table of contents ont the right panel of the post, can turn it off globally in _config.yml
---


## PLUGINS

### `markdown-preview.nvim`
- Looks fine
- Use Browser to preview the content
- Update as you type

### `semshi`
- Has issue of `:Semshi not found`, will try to fix it later

### `SmoothCursor.nvim`
- visual effect with `matrix` type

### `toggleterm.nvim`
- Has issue of loading profile for `pwsh`, will try to fix it later

### `nimv-spectre`
- searching and replacing

### `nvim-markdown`
- markdown highlighting and concealing, etc.
- use `vim.g.vim_markdown_xxx` to configure it
- use `vim.api.nvim_set_keymap` to set keymap or disable certain keymap

----

## KEYBINDS

### `toggleterm`
- `<C-\>` to open terminal
- `<leader>t+`, `t`, `h`, `f`, `v` for toggle terminal in `tab`, `horizontal`, `float`, `vertical` view
- after escape in terminal, `#<C-\>` to open an additional terminal

