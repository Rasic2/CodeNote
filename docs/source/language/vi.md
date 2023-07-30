# Vim 教程

## 配置

```vim
set nu
syntax on
set hlsearch
set background=dark
colorscheme solarized
set nocompatible
set backspace=indent,eol,start

" Uncomment the following to have Vim jump to the last position when
" reopening a file
if has("autocmd")
  au BufReadPost * if line("'\"") > 1 && line("'\"") <= line("$") | exe "normal! g'\"" | endif
endif
```

:::{note}
colorscheme 需手动下载，地址在[这](https://vimcolorschemes.com/top)
:::

## 高亮显示光标所属词

```
g + d
```
