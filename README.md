" Pathogen load plugins
set nocompatible
filetype off
execute pathogen#infect()
execute pathogen#helptags()
filetype plugin indent on
filetype plugin on
filetype on
syntax on

let g:pymode_python = 'python3'
if has("autocmd")
  autocmd Filetype java setlocal omnifunc=javacomplete#Complete
  autocmd Filetype java setlocal completefunc=javacomplete#CompleteParamsInfo
endif

" neocomplete
" Disable AutoComplPop.
let g:acp_enableAtStartup = 1 
" Use neocomplete.
let g:neocomplete#enable_at_startup = 1
" Use smartcase.
let g:neocomplete#enable_smart_case = 1
" Set minimum syntax keyword length.
let g:neocomplete#sources#syntax#min_keyword_length = 3
let g:neocomplete#lock_buffer_name_pattern = '\*ku\*'
" Define keyword.
if !exists('g:neocomplete#keyword_patterns')
    let g:neocomplete#keyword_patterns = {}
endif
let g:neocomplete#keyword_patterns['default'] = '\h\w*'

" Plugin key-mappings.
inoremap <expr><C-g>     neocomplete#undo_completion()
inoremap <expr><C-l>     neocomplete#complete_common_string()

" AutoComplPop like behavior.
" let g:neocomplete#enable_auto_select = 1
" Enable omni completion.
if has("autocmd")
  autocmd FileType css setlocal omnifunc=csscomplete#CompleteCSS
  autocmd FileType html,markdown setlocal omnifunc=htmlcomplete#CompleteTags
  autocmd FileType javascript setlocal omnifunc=javascriptcomplete#CompleteJS
  autocmd FileType python setlocal omnifunc=python3complete#Complete
  autocmd FileType xml setlocal omnifunc=xmlcomplete#CompleteTags
endif

" Enable heavy omni completion.
if !exists('g:neocomplete#sources#omni#input_patterns')
  let g:neocomplete#sources#omni#input_patterns = {}
endif
"let g:neocomplete#sources#omni#input_patterns.php = '[^. \t]->\h\w*\|\h\w*::'
"let g:neocomplete#sources#omni#input_patterns.c = '[^.[:digit:] *\t]\%(\.\|->\)'
"let g:neocomplete#sources#omni#input_patterns.cpp = '[^.[:digit:] *\t]\%(\.\|->\)\|\h\w*::'

" end of neocomplete

" folding xml file
augroup XML
    autocmd!
    autocmd FileType xml setlocal foldmethod=indent foldlevelstart=999 foldminlines=0
augroup END

set laststatus=2
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
set gfn=Consolas:h10:cDEFAULT
let Tlist_Ctags_Cmd="D:/K/Vim/plugins/ctags58/ctags.exe"
let Tlist_Use_Right_Window = 1
"let g:ctrlp_max_files=0
"let g:ctrlp_max_depth=90
"let g:ctrlp_working_path_mode=''
let g:SuperTabDefaultCompletionType = "<c-n>"
"filetype on
"filetype plugin on
set omnifunc=syntaxcomplete#Complete
let g:tagbar_ctags_bin="D:/K/Vim/plugins/ctags58/ctags.exe"
let g:airline_extensions=[]
source $VIMRUNTIME/vimrc_example.vim
source $VIMRUNTIME/mswin.vim

behave mswin
colorscheme solarized 
nmap <F7> :NERDTreeToggle<CR>
nmap <F8> :TagbarToggle<CR>

" for ssh edit
let g:netrw_cygwin = 0
let g:netrw_list_cmd = "D:/K/tools/plink.exe -P 27903 -pw 1Mon1kzk fanqiang@23.83.250.19 ls -Fa "
let g:netrw_ssh_cmd  = "D:/K/tools/plink.exe -T -ssh"
let g:netrw_scp_cmd  = "D:/K/tools/pscp.exe -P 27903 -pw 1Mon1kzk -scp"
let g:netrw_sftp_cmd = "D:/K/tools/pscp.exe -pw 1Mon1kzk -sftp"

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
set noundofile
