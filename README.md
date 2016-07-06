# vimrc
let g:pymode_python = 'python3'
if has("autocmd")
  autocmd Filetype java setlocal omnifunc=javacomplete#Complete
  autocmd Filetype java setlocal completefunc=javacomplete#CompleteParamsInfo
endif

set laststatus=2
set nocompatible
set number
set clipboard+=unnamed  " use the clipboards of vim and win
set paste               " Paste from a windows or from vim
set autoindent
set smartindent
set tabstop=4
set shiftwidth=4
set showmatch
set incsearch
set encoding=utf-8  " The encoding displayed.
set fileencoding=utf-8  " The encoding written to file.
set runtimepath^="D:\Program Files (x86)/Vim/vim74/plugin/ctrlp.vim"
set gfn=Consolas:h10:cDEFAULT
let Tlist_Ctags_Cmd="D:/K/Vim/plugins/ctags58/ctags.exe"
let Tlist_Use_Right_Window = 1
let g:ctrlp_max_files=0
let g:ctrlp_max_depth=90
let g:ctrlp_working_path_mode=''
let g:SuperTabDefaultCompletionType = "<c-n>"
"filetype on
"filetype plugin on
set omnifunc=syntaxcomplete#Complete
let g:tagbar_ctags_bin="D:/K/Vim/plugins/ctags58/ctags.exe"
let g:airline_extensions=[]
source $VIMRUNTIME/vimrc_example.vim
source $VIMRUNTIME/mswin.vim

" for python mode
" Pathogen load
filetype off

call pathogen#infect()
call pathogen#helptags()

filetype plugin indent on
syntax on
behave mswin
colorscheme solarized 
nmap <F7> :NERDTreeToggle<CR>
nmap <F8> :TagbarToggle<CR>
set diffexpr=MyDiff()
function MyDiff()
  let opt = '-a --binary '
  if &diffopt =~ 'icase' | let opt = opt . '-i ' | endif
  if &diffopt =~ 'iwhite' | let opt = opt . '-b ' | endif
  let arg1 = v:fname_in
  if arg1 =~ ' ' | let arg1 = '"' . arg1 . '"' | endif
  let arg2 = v:fname_new
  if arg2 =~ ' ' | let arg2 = '"' . arg2 . '"' | endif
  let arg3 = v:fname_out
  if arg3 =~ ' ' | let arg3 = '"' . arg3 . '"' | endif
  let eq = ''
  if $VIMRUNTIME =~ ' '
    if &sh =~ '\<cmd'
      let cmd = '""' . $VIMRUNTIME . '\diff"'
      let eq = '"'
    else
      let cmd = substitute($VIMRUNTIME, ' ', '" ', '') . '\diff"'
    endif
  else
    let cmd = $VIMRUNTIME . '\diff'
  endif
  silent execute '!' . cmd . ' ' . opt . arg1 . ' ' . arg2 . ' > ' . arg3 . eq
endfunction

function! MakeSession()
  let b:sessiondir = "D:/K/Vim/sessions"
  if (filewritable(b:sessiondir) != 2)
    exe 'silent !mkdir -p ' b:sessiondir
    redraw!
  endif
  let b:filename = b:sessiondir . '/session.vim'
  exe "mksession! " . b:filename
endfunction

function! LoadSession()
  let b:sessiondir = "D:/K/Vim/sessions"
  let b:sessionfile = b:sessiondir . "/session.vim"
  if (filereadable(b:sessionfile))
    exe 'source ' b:sessionfile
  else
    echo "No session loaded."
  endif
endfunction

" Adding automatons for when entering or leaving Vim
if(argc() == 0)
	au VimEnter * nested :call LoadSession()
endif
au VimLeave * :call MakeSession()

set nobackup
set nowritebackup
set noswapfile

