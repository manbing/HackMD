---
title: vim
tags: [Linux]

---

# Status
normal mode
insert mode

# vim.rc
```
set cursorline
set ai
set bg=light
set shiftwidth=8
set tabstop=8
set mouse=a
set number
set cindent
set autoindent
set smartindent
set nocompatible
set nopaste
set bs=2
set ic

syntax on
filetype on

highlight Search term=reverse ctermbg=4 ctermfg=7
highlight Normal ctermbg=black ctermfg=white
highlight Folded ctermbg=black ctermfg=darkcyan
hi Comment ctermbg=black ctermfg=darkcyan
color desert

" 下面出現一列 bar.
set ls=2
set statusline=%<%f\ %m%=\ %h%r\ %-19([%p%%]\ %3l,%02c%03V%)%y
highlight StatusLine ctermfg=blue ctermbg=white

" 搜尋到的字加 hilight
set hlsearch



" :sp 開檔時, 上面會列出所有檔案
set wildmenu

" 可以用 {{{ }}} 縮排 Folded
set foldmethod=marker

" Handle tmux $TERM quirks in vim
if $TERM =~ '^screen-256color'
map <Esc>OH <Home>
map! <Esc>OH <Home>
map <Esc>OF <End>
map! <Esc>OF <End>
endif

" 會自動到最後離開的位置
if has("autocmd")
    autocmd BufRead *.txt set tw=78
    autocmd BufReadPost * if line("'\"") > 0 && line ("'\"") <= line("$") | exe "normal g'\"" | endif
endif

if has("cscope")
        set csprg=/usr/bin/cscope
        set csto=0
        set cst
        set nocsverb
        " add any database in current directory
        if filereadable("cscope.out")
            cs add cscope.out
        " else add database pointed to by environment
        elseif $CSCOPE_DB != ""
            cs add $CSCOPE_DB
        endif
        set csverb
endif

nnoremap <silent> <F12> :TlistOpen<CR>
nnoremap <silent> <F11> :NERDTree<CR>
nnoremap <silent> <F10> :MRU<CR>
nnoremap <silent>  ff  :FufFile<CR>
nnoremap <silent>  fb  :FufBuffer<CR>
nnoremap <silent>  fm  :FufMruCmd<CR>
nnoremap <silent> <F9> :set paste<CR>

nmap <C-\>s :cs find s <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>g :cs find g <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>c :cs find c <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>t :cs find t <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>e :cs find e <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>f :cs find f <C-R>=expand("<cfile>")<CR><CR>
nmap <C-\>i :cs find i <C-R>=expand("<cfile>")<CR><CR>
nmap <C-\>d :cs find d <C-R>=expand("<cword>")<CR><CR>
```

# Plugin
[Vundle](https://github.com/VundleVim/Vundle.vim)
:PluginList
:PluginInstall

[coc.nvim](https://github.com/neoclide/coc.nvim)
:CocInstall coc-clangd
:CocInstall coc-rust-analyzer
:CocInstall coc-snippets
 
# Shortcuts
## Cursor (normal mode)
h: move left
j: move down
k: move up
l: move right
gg: move top of file
G: move bottom
W: move forward by one word
b: move back by one word
0: begin of the line
$: end of the line
    
## Delete
```
d+w
d+i+<,(, ", }
```

# Replace

```
Replace string, IP="140.134.4.1", as string, IP="0.0.0.0"

:%s/IP="[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}"/IP="0.0.0.0"/g
```

## Reference
[Lecture 3: Editors (vim) (2020)](https://www.youtube.com/watch?v=a6Q8Na575qc)


# Status
normal mode
insert mode

# vim.rc
```
set cursorline
set ai
set bg=light
set shiftwidth=8
set tabstop=8
set mouse=a
set number
set cindent
set autoindent
set smartindent
set nocompatible
set nopaste
set bs=2
set ic

syntax on
filetype on

highlight Search term=reverse ctermbg=4 ctermfg=7
highlight Normal ctermbg=black ctermfg=white
highlight Folded ctermbg=black ctermfg=darkcyan
hi Comment ctermbg=black ctermfg=darkcyan
color desert

" 下面出現一列 bar.
set ls=2
set statusline=%<%f\ %m%=\ %h%r\ %-19([%p%%]\ %3l,%02c%03V%)%y
highlight StatusLine ctermfg=blue ctermbg=white

" 搜尋到的字加 hilight
set hlsearch



" :sp 開檔時, 上面會列出所有檔案
set wildmenu

" 可以用 {{{ }}} 縮排 Folded
set foldmethod=marker

" Handle tmux $TERM quirks in vim
if $TERM =~ '^screen-256color'
map <Esc>OH <Home>
map! <Esc>OH <Home>
map <Esc>OF <End>
map! <Esc>OF <End>
endif

" 會自動到最後離開的位置
if has("autocmd")
    autocmd BufRead *.txt set tw=78
    autocmd BufReadPost * if line("'\"") > 0 && line ("'\"") <= line("$") | exe "normal g'\"" | endif
endif

if has("cscope")
        set csprg=/usr/bin/cscope
        set csto=0
        set cst
        set nocsverb
        " add any database in current directory
        if filereadable("cscope.out")
            cs add cscope.out
        " else add database pointed to by environment
        elseif $CSCOPE_DB != ""
            cs add $CSCOPE_DB
        endif
        set csverb
endif

nnoremap <silent> <F12> :TlistOpen<CR>
nnoremap <silent> <F11> :NERDTree<CR>
nnoremap <silent> <F10> :MRU<CR>
nnoremap <silent>  ff  :FufFile<CR>
nnoremap <silent>  fb  :FufBuffer<CR>
nnoremap <silent>  fm  :FufMruCmd<CR>
nnoremap <silent> <F9> :set paste<CR>

nmap <C-\>s :cs find s <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>g :cs find g <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>c :cs find c <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>t :cs find t <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>e :cs find e <C-R>=expand("<cword>")<CR><CR>
nmap <C-\>f :cs find f <C-R>=expand("<cfile>")<CR><CR>
nmap <C-\>i :cs find i <C-R>=expand("<cfile>")<CR><CR>
nmap <C-\>d :cs find d <C-R>=expand("<cword>")<CR><CR>
```

# Plugin
[Vundle](https://github.com/VundleVim/Vundle.vim)
:PluginList
:PluginInstall

[coc.nvim](https://github.com/neoclide/coc.nvim)
:CocInstall coc-clangd
:CocInstall coc-rust-analyzer
:CocInstall coc-snippets
 
# Shortcuts
## Cursor (normal mode)
h: move left
j: move down
k: move up
l: move right
gg: move top of file
G: move bottom
W: move forward by one word
b: move back by one word
0: begin of the line
$: end of the line
    
## Delete
```
d+w
d+i+<,(, ", }
```

# Replace

```
Replace string, IP="140.134.4.1", as string, IP="0.0.0.0"

:%s/IP="[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}"/IP="0.0.0.0"/g
```

## Reference
[Lecture 3: Editors (vim) (2020)](https://www.youtube.com/watch?v=a6Q8Na575qc)


