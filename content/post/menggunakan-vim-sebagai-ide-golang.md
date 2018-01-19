---
date: 2017-01-14T18:31:18+07:00
lastmod: 2017-01-14T18:31:18+07:00

title: "Menggunakan Vim Sebagai IDE Golang"
description: "Menggunakan Vim Sebagai IDE Golang"
author: "Imam Digmi"

weight: 50
draft: true
keywords: ["golang", "go", "vim", "ide"]
tags: []
categories: ["Vim", "GoLang"]

comment: true
toc: true
autoCollapseToc: false
contentCopyright: <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">CC BY-NC-ND 4.0</a>
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
---

Untuk menggunakan Vim (http://www.vim.org), ada plugin utama serta berbagai plugin pendukung yang bisa digunakan. Sebaiknya, menggunakan pathogen untuk mempermudah pengelolaan berbagai plugin tersebut. Bagian ini akan menjelaskan berbagai konfigurasi serta instalasi yang diperlukan sehingga Vim bisa menjadi peranti untuk pengembangan aplikasi menggunakan Go. <!-- more -->

## Instalasi dan Konfigurasi Pathogen
Pathogen adalah plugin dari Tim Pope yang digunakan untuk mempermudah pengelolaan plugin. Kode sumber dari Pathogen bisa diperoleh di [vim-pathogen](https://github.com/tpope/vim-pathogen) . Untuk instalasi, ikuti langkah berikut:

```bash
cd
mkdir .vim/autoload
mkdir .vim/bundle
cd .vim/autoload
wget https://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim
```

Output

```
--2017-01-14 18:34:05--  https://raw.github.com/tpope/vim-pathogen/master/autoload/pathogen.vim
Resolving raw.github.com (raw.github.com)... 151.101.100.133
Connecting to raw.github.com (raw.github.com)|151.101.100.133|:443... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim [following]
--2017-01-14 18:34:06--  https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.100.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.100.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 12474 (12K) [text/plain]

Saving to: ‘pathogen.vim’

pathogen.vim.1	100%[==========================================================================================================>]  12,18K  --.-KB/s    in 0s

2017-01-14 18:34:07 (115 MB/s) - ‘pathogen.vim.1’ saved [12474/12474]
```
Lalu kita lihat hasil download tadi

```bash
ls -la
```

Output

```bash
drwxr-xr-x 2 idiot idiot 4,0K Jan 14 18:35 .
drwxr-xr-x 4 idiot idiot 4,0K Jan 10 00:50 ..
-rw-r--r-- 1 idiot idiot  13K Jan 10 00:43 pathogen.vim
```

Setelah itu, untuk menggunakan Pathogen, letakkan aktivasinya di `$HOME/.vimrc` atau di `$HOME/.vim/vimrc` (saya piliah lokasi yang kedua) sebagai berikut:

```bash
execute pathogen#infect()
```

Setelah itu, semua plugin tinggal kita ambil dari repository (bisa dari github, bitbucket, dan lain-lain) langsung di-copy satu direktori ke direktori `$HOME/.vim/bundle`.

## Instalasi dan Kofigurasi Plugin Golang dan Plugin Pendukung
Setelah selesai melakukan instalasi Pathogen, berbagai plugin yang diperlukan bisa diambil langsung dari Internet. Berikut ini adalah daftar yang digunakan penulis:

1. **Colorschemes**: untuk tema warna dari Vim. Bisa diperoleh di [vim-colorschemes](https://github.com/flazz/vim-colorschemes)
2. **Nerdtree**: untuk menampilkan file-file dalam struktur pohon di sebelah kiri sehingga memudahkan navigasi. Bisa diperoleh di [nerdtree](https://github.com/scrooloose/nerdtree)
3. **Golang**: plugin utama agar Vim mengenali kode sumber Go, bisa diperoleh di [vim-golang](https://github.com/jnwhiteh/vim-golang)
Cara instalasi:

```bash
cd $HOME/.vim/bundle
git clone <plugin-repo>
```

Dan ada satu lagi yaitu color scheme walaupun ini bersifat opsional tapi ini sangat disarankan agar vim kita terlihat cantik, cara menambahkannya seperti ini :

```bash
mkdir ~/.vim/colors
mkdir ~/.vim/tmp
mkdir ~/.vim/tmp/undo
mkdir ~/.vim/tmp/backup
mkdir ~/.vim/tmp/swap
```

Copy code dibawah ini lalu simpan pada folder `~/.vim/colors` dan beri nama `molokai.vim` :

```
" Vim color file
"
" Author: Tomas Restrepo <tomas@winterdom.com>
" https://github.com/tomasr/molokai
"
" Note: Based on the Monokai theme for TextMate
" by Wimer Hazenberg and its darker variant
" by Hamish Stuart Macpherson
"

hi clear

if version > 580
    " no guarantees for version 5.8 and below, but this makes it stop
    " complaining
    hi clear
    if exists("syntax_on")
        syntax reset
    endif
endif
let g:colors_name="molokai"

if exists("g:molokai_original")
    let s:molokai_original = g:molokai_original
else
    let s:molokai_original = 0
endif


hi Boolean         guifg=#AE81FF
hi Character       guifg=#E6DB74
hi Number          guifg=#AE81FF
hi String          guifg=#E6DB74
hi Conditional     guifg=#F92672               gui=bold
hi Constant        guifg=#AE81FF               gui=bold
hi Cursor          guifg=#000000 guibg=#F8F8F0
hi iCursor         guifg=#000000 guibg=#F8F8F0
hi Debug           guifg=#BCA3A3               gui=bold
hi Define          guifg=#66D9EF
hi Delimiter       guifg=#8F8F8F
hi DiffAdd                       guibg=#13354A
hi DiffChange      guifg=#89807D guibg=#4C4745
hi DiffDelete      guifg=#960050 guibg=#1E0010
hi DiffText                      guibg=#4C4745 gui=italic,bold

hi Directory       guifg=#A6E22E               gui=bold
hi Error           guifg=#E6DB74 guibg=#1E0010
hi ErrorMsg        guifg=#F92672 guibg=#232526 gui=bold
hi Exception       guifg=#A6E22E               gui=bold
hi Float           guifg=#AE81FF
hi FoldColumn      guifg=#465457 guibg=#000000
hi Folded          guifg=#465457 guibg=#000000
hi Function        guifg=#A6E22E
hi Identifier      guifg=#FD971F
hi Ignore          guifg=#808080 guibg=bg
hi IncSearch       guifg=#C4BE89 guibg=#000000

hi Keyword         guifg=#F92672               gui=bold
hi Label           guifg=#E6DB74               gui=none
hi Macro           guifg=#C4BE89               gui=italic
hi SpecialKey      guifg=#66D9EF               gui=italic

hi MatchParen      guifg=#000000 guibg=#FD971F gui=bold
hi ModeMsg         guifg=#E6DB74
hi MoreMsg         guifg=#E6DB74
hi Operator        guifg=#F92672

" complete menu
hi Pmenu           guifg=#66D9EF guibg=#000000
hi PmenuSel                      guibg=#808080
hi PmenuSbar                     guibg=#080808
hi PmenuThumb      guifg=#66D9EF

hi PreCondit       guifg=#A6E22E               gui=bold
hi PreProc         guifg=#A6E22E
hi Question        guifg=#66D9EF
hi Repeat          guifg=#F92672               gui=bold
hi Search          guifg=#000000 guibg=#FFE792
" marks
hi SignColumn      guifg=#A6E22E guibg=#232526
hi SpecialChar     guifg=#F92672               gui=bold
hi SpecialComment  guifg=#7E8E91               gui=bold
hi Special         guifg=#66D9EF guibg=bg      gui=italic
if has("spell")
    hi SpellBad    guisp=#FF0000 gui=undercurl
    hi SpellCap    guisp=#7070F0 gui=undercurl
    hi SpellLocal  guisp=#70F0F0 gui=undercurl
    hi SpellRare   guisp=#FFFFFF gui=undercurl
endif
hi Statement       guifg=#F92672               gui=bold
hi StatusLine      guifg=#455354 guibg=fg
hi StatusLineNC    guifg=#808080 guibg=#080808
hi StorageClass    guifg=#FD971F               gui=italic
hi Structure       guifg=#66D9EF
hi Tag             guifg=#F92672               gui=italic
hi Title           guifg=#ef5939
hi Todo            guifg=#FFFFFF guibg=bg      gui=bold

hi Typedef         guifg=#66D9EF
hi Type            guifg=#66D9EF               gui=none
hi Underlined      guifg=#808080               gui=underline

hi VertSplit       guifg=#808080 guibg=#080808 gui=bold
hi VisualNOS                     guibg=#403D3D
hi Visual                        guibg=#403D3D
hi WarningMsg      guifg=#FFFFFF guibg=#333333 gui=bold
hi WildMenu        guifg=#66D9EF guibg=#000000

hi TabLineFill     guifg=#1B1D1E guibg=#1B1D1E
hi TabLine         guibg=#1B1D1E guifg=#808080 gui=none

if s:molokai_original == 1
   hi Normal          guifg=#F8F8F2 guibg=#272822
   hi Comment         guifg=#75715E
   hi CursorLine                    guibg=#3E3D32
   hi CursorLineNr    guifg=#FD971F               gui=none
   hi CursorColumn                  guibg=#3E3D32
   hi ColorColumn                   guibg=#3B3A32
   hi LineNr          guifg=#BCBCBC guibg=#3B3A32
   hi NonText         guifg=#75715E
   hi SpecialKey      guifg=#75715E
else
   hi Normal          guifg=#F8F8F2 guibg=#1B1D1E
   hi Comment         guifg=#7E8E91
   hi CursorLine                    guibg=#293739
   hi CursorLineNr    guifg=#FD971F               gui=none
   hi CursorColumn                  guibg=#293739
   hi ColorColumn                   guibg=#232526
   hi LineNr          guifg=#465457 guibg=#232526
   hi NonText         guifg=#465457
   hi SpecialKey      guifg=#465457
end

"
" Support for 256-color terminal
"
if &t_Co > 255
   if s:molokai_original == 1
      hi Normal                   ctermbg=234
      hi CursorLine               ctermbg=235   cterm=none
      hi CursorLineNr ctermfg=208               cterm=none
   else
      hi Normal       ctermfg=252 ctermbg=233
      hi CursorLine               ctermbg=234   cterm=none
      hi CursorLineNr ctermfg=208               cterm=none
   endif
   hi Boolean         ctermfg=135
   hi Character       ctermfg=144
   hi Number          ctermfg=135
   hi String          ctermfg=144
   hi Conditional     ctermfg=161               cterm=bold
   hi Constant        ctermfg=135               cterm=bold
   hi Cursor          ctermfg=16  ctermbg=253
   hi Debug           ctermfg=225               cterm=bold
   hi Define          ctermfg=81
   hi Delimiter       ctermfg=241

   hi DiffAdd                     ctermbg=24
   hi DiffChange      ctermfg=181 ctermbg=239
   hi DiffDelete      ctermfg=162 ctermbg=53
   hi DiffText                    ctermbg=102 cterm=bold

   hi Directory       ctermfg=118               cterm=bold
   hi Error           ctermfg=219 ctermbg=89
   hi ErrorMsg        ctermfg=199 ctermbg=16    cterm=bold
   hi Exception       ctermfg=118               cterm=bold
   hi Float           ctermfg=135
   hi FoldColumn      ctermfg=67  ctermbg=16
   hi Folded          ctermfg=67  ctermbg=16
   hi Function        ctermfg=118
   hi Identifier      ctermfg=208               cterm=none
   hi Ignore          ctermfg=244 ctermbg=232
   hi IncSearch       ctermfg=193 ctermbg=16

   hi keyword         ctermfg=161               cterm=bold
   hi Label           ctermfg=229               cterm=none
   hi Macro           ctermfg=193
   hi SpecialKey      ctermfg=81

   hi MatchParen      ctermfg=233  ctermbg=208 cterm=bold
   hi ModeMsg         ctermfg=229
   hi MoreMsg         ctermfg=229
   hi Operator        ctermfg=161

   " complete menu
   hi Pmenu           ctermfg=81  ctermbg=16
   hi PmenuSel        ctermfg=255 ctermbg=242
   hi PmenuSbar                   ctermbg=232
   hi PmenuThumb      ctermfg=81

   hi PreCondit       ctermfg=118               cterm=bold
   hi PreProc         ctermfg=118
   hi Question        ctermfg=81
   hi Repeat          ctermfg=161               cterm=bold
   hi Search          ctermfg=0   ctermbg=222   cterm=NONE

   " marks column
   hi SignColumn      ctermfg=118 ctermbg=235
   hi SpecialChar     ctermfg=161               cterm=bold
   hi SpecialComment  ctermfg=245               cterm=bold
   hi Special         ctermfg=81
   if has("spell")
       hi SpellBad                ctermbg=52
       hi SpellCap                ctermbg=17
       hi SpellLocal              ctermbg=17
       hi SpellRare  ctermfg=none ctermbg=none  cterm=reverse
   endif
   hi Statement       ctermfg=161               cterm=bold
   hi StatusLine      ctermfg=238 ctermbg=253
   hi StatusLineNC    ctermfg=244 ctermbg=232
   hi StorageClass    ctermfg=208
   hi Structure       ctermfg=81
   hi Tag             ctermfg=161
   hi Title           ctermfg=166
   hi Todo            ctermfg=231 ctermbg=232   cterm=bold

   hi Typedef         ctermfg=81
   hi Type            ctermfg=81                cterm=none
   hi Underlined      ctermfg=244               cterm=underline

   hi VertSplit       ctermfg=244 ctermbg=232   cterm=bold
   hi VisualNOS                   ctermbg=238
   hi Visual                      ctermbg=235
   hi WarningMsg      ctermfg=231 ctermbg=238   cterm=bold
   hi WildMenu        ctermfg=81  ctermbg=16

   hi Comment         ctermfg=59
   hi CursorColumn                ctermbg=236
   hi ColorColumn                 ctermbg=236
   hi LineNr          ctermfg=250 ctermbg=236
   hi NonText         ctermfg=59

   hi SpecialKey      ctermfg=59

   if exists("g:rehash256") && g:rehash256 == 1
       hi Normal       ctermfg=252 ctermbg=234
       hi CursorLine               ctermbg=236   cterm=none
       hi CursorLineNr ctermfg=208               cterm=none

       hi Boolean         ctermfg=141
       hi Character       ctermfg=222
       hi Number          ctermfg=141
       hi String          ctermfg=222
       hi Conditional     ctermfg=197               cterm=bold
       hi Constant        ctermfg=141               cterm=bold

       hi DiffDelete      ctermfg=125 ctermbg=233

       hi Directory       ctermfg=154               cterm=bold
       hi Error           ctermfg=222 ctermbg=233
       hi Exception       ctermfg=154               cterm=bold
       hi Float           ctermfg=141
       hi Function        ctermfg=154
       hi Identifier      ctermfg=208

       hi Keyword         ctermfg=197               cterm=bold
       hi Operator        ctermfg=197
       hi PreCondit       ctermfg=154               cterm=bold
       hi PreProc         ctermfg=154
       hi Repeat          ctermfg=197               cterm=bold

       hi Statement       ctermfg=197               cterm=bold
       hi Tag             ctermfg=197
       hi Title           ctermfg=203
       hi Visual                      ctermbg=238

       hi Comment         ctermfg=244
       hi LineNr          ctermfg=239 ctermbg=235
       hi NonText         ctermfg=239
       hi SpecialKey      ctermfg=239
   endif
end

" Must be at the end, because of ctermbg=234 bug.
" https://groups.google.com/forum/#!msg/vim_dev/afPqwAFNdrU/nqh6tOM87QUJ
set background=dark
```

## Powerline
install plugin vim powerline, pastikan sudah install git
clone dulu repo powerline

```bash
git clone git://github.com/Lokaltog/vim-powerline.git ~/.vim/bundle/powerline
```

## Pumbers
Untuk numbering kita membutuhkan plugins lagi, langsung saja clone numbers vim nya :

```bash
git clone https://github.com/myusuf3/numbers.vim.git ~/.vim/bundle/numbers
```

Hasil dari menjalankan `vim` melalui shell untuk menulis kode sumber Go bisa dilihat pada gambar berikut:

![Figure 1](/menggunakan-vim-sebagai-ide-golang/1.png)

## Autocompletion
Vim menyediakan fasilitas autocompletion melalui Omniautocompletion. Fasilitas ini sudah terinstall saat kita menginstall Vim. Untuk mengaktifkan fasilitas ini untuk keperluan Go, kita harus menginstall dan mengaktifkan Gocode (https://github.com/nsf/gocode). Sebaiknya kode sumber dari Gocode diambil semua karena ada script Vim yang akan kita gunakan:

```bash
git clone https://github.com/nsf/gocode.git
```

Setelah itu, install Gocode menggunakan perintah `go get -u github.com/nsf/gocode`. Hasilnya adalah file executable binary $GOROOT/bin/gocode. Sebelum menggunakan Vim, aktifkan dulu gocode dengan mengeksekusi gocode melalui shell. Setelah itu, tambahkan satu baris di $HOME/.vim/vimrc: set ofu=syntaxcomplete#Complete di bawah baris filetype plugin indent on.
Kode sumber lengkap dari `$HOME/.vim/vimrc` yang penulis gunakan bisa dilihat pada Listing berikut:

```
set encoding=utf-8
set laststatus=2
set number
set smartindent
set tabstop=4
set shiftwidth=4
set softtabstop=4
set expandtab
set colorcolumn=+1
set cursorline              " highligh current line
set ignorecase
set smartcase
set showmatch
set hlsearch
set gdefault
set virtualedit=block
set cpoptions+=n
set ofu=syntaxcomplete#Complete

filetype plugin indent on

" colors
colorscheme molokai
syntax on
set t_Co=256
set guifont=DejaVu\ Sans\ Mono\ for\ Powerline\ 10
let Powerline_symbols = 'fancy'

inoremap <c-a> <esc>I
inoremap <c-e> <esc>A

" Return to the previously selected line in the file
augroup line_return
    au!
    au BufReadPost *
        \ if line("'\"") > 0 && line("'\"") <= line("$") |
        \ execute 'normal! g`"zvzz' |
        \ endif
augroup END

" remove seach-highlights when pressing ,+<space>
let mapleader=","
noremap <leader><space> :noh<cr>: call clearmatches()<cr>

" Buffers, Backup & Co
set undodir=~/.vim/tmp/undo//
set backupdir=~/.vim/tmp/backup//
set directory=~/.vim/tmp/swap//
set backup
set noswapfile
set ruler
set history=10000
set undofile
set undoreload=10000
set title
set autoindent
set autoread
set lazyredraw
set mouse=a

" reload .vimrc on saving
au BufWritePost .vimrc so ~/.vim/vimrc

execute pathogen#infect()
call pathogen#infect('bundle/{}')

nnoremap <F3> :NumbersToggle<CR>
nnoremap <F4> :NumbersOnOff<CR>

" Original monokai background color
let g:molokai_original = 1

" Nerdtree Setup
autocmd vimenter * NERDTree
autocmd vimenter * if !argc() | NERDTree | endif
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif
let g:NERDTreeDirArrows=0

let g:cssColorVimDoNotMessMyUpdatetime = 1
highlight LineNr term=bold cterm=NONE ctermfg=DarkGrey ctermbg=NONE gui=NONE guifg=DarkGrey guibg=NONE
set grepprg=grep\ -nH\ $*

" Fix bug 256color from tmux
if &term =~ '256color'
  " disable Background Color Erase (BCE)
  set t_ut=
endif
```

Untuk mengaktifkan completion, kita harus masuk ke mode Insert dari Vim, setelah itu tekan Ctrl-X, Ctrl-O secara cepat. Hasil autocompletion bisa dilihat di gambar berikut:
![Figure 2](/menggunakan-vim-sebagai-ide-golang/2.png)