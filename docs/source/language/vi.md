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

" add tab space
set ts=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent

set ruler               " 在编辑过程中，在右下角显示光标位置的状态行
set laststatus=2        " 显示状态栏 (默认值为 1, 无法显示状态栏)
set statusline=\ %<%F[%1*%M%*%n%R%H]%=\ %y\ %0(%{&fileformat}\ %{&encoding}\ %c:%l/%L%)\    " 设置在状态行显示的信息

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
