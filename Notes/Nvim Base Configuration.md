Install Neovim:
Make sure you have Neovim installed on your system. You can download it from the official website or use a package manager like brew (on macOS) or apt (on Ubuntu).

Configuration file:
Create or edit the Neovim configuration file init.vim (or init.lua if you prefer Lua-based configuration) in the appropriate directory:

On Unix-like systems (Linux, macOS): ~/.config/nvim/init.vim
On Windows: %USERPROFILE%\AppData\Local\nvim\init.vim
Base Configuration:
Here's a simple base config to start with:

```vim
" Basic settings
set number                   " Show line numbers
set relativenumber           " Show relative line numbers
set tabstop=4                " Set tab width to 4 spaces
set shiftwidth=4             " Set the number of spaces to use for autoindent
set expandtab                " Use spaces instead of tabs
set smartindent              " Enable smart indenting
set autoindent               " Enable auto indenting
set nowrap                   " Disable line wrapping

" Enable syntax highlighting
syntax enable

" Enable line highlighting
set cursorline

" Enable mouse support
set mouse=a

" Enable clipboard support
set clipboard=unnamedplus

" Enable incremental search
set incsearch

" Enable line and column numbers in the status line
set statusline=%<%f\ %h%m%r%=%b\ (%l,%c%V)\ \ \ %P
set laststatus=2

" Use the dark theme
set background=dark

" Disable swap and backup files
set noswapfile
set nobackup
set nowritebackup

" Enable searching as you type
set incsearch
set hlsearch

" Enable persistent undo
set undofile

" Key mappings
" Save with <Leader>s (Leader is set to \ by default)
let mapleader = "\<Space>"

nnoremap <Leader>s :w<CR>
vnoremap <Leader>s :w<CR>

" Quit with <Leader>q
nnoremap <Leader>q :q<CR>

" Save and quit with <Leader>wq
nnoremap <Leader>wq :wq<CR>

" Reload the configuration file with <Leader>r
nnoremap <Leader>r :source ~/.config/nvim/init.vim<CR>

nnoremap <Leader>jk <Esc><CR>
```
