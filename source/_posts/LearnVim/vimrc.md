---
title: vim配置
---
" ----------------------------- Vundle Start -----------------------------
set nocompatible
filetype off
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
"插件管理
Plugin 'VundleVim/Vundle.vim'
"目录树
Plugin 'The-NERD-tree'
"代码补全
Bundle 'Valloric/YouCompleteMe'
"异步语法检查
Plugin 'w0rp/ale'
call vundle#end()
filetype plugin indent on
" ----------------------------- Vundle End   -----------------------------


" 开启语法高亮
syntax on

" 设置字体
set guifont=courier\ 20

" 检测文件类型
filetype on

" 设置取消备份，禁止临时文件生成
set nobackup
set noswapfile

" 设置在Vim中可以使用鼠标，防止终端无法拷贝
set mouse=a

" 显示当前行号和列号
set ruler

" 在状态栏显示正在输入的命令
set showcmd

" 总是显示状态栏(Powerline需要2行)
set laststatus=2

" 显示行号
set number

" 设置代码匹配,包括括号匹配情况
set showmatch

" 设置C/C++方式自动对齐
set autoindent
set cindent
set smartindent

" 设置tab宽度
set tabstop=4

" 设置自动对齐空格数
set shiftwidth=4

" 按退格键时可以一次删除4个空格
set softtabstop=4

" 自动补全配置让Vim补全菜单行为跟IDE一致
set completeopt=longest,menu

" 增强模式中的命令行自动完成操作
set wildmenu




map <F5> :NERDTreeToggle<CR>
"map <C-n> :NERDTreeToggle<CR>
" 显示行号
let NERDTreeShowLineNumbers=1
let NERDTreeAutoCenter=1
" 是否显示隐藏文件
"let NERDTreeShowHidden=1
" 设置宽度
let NERDTreeWinSize=30
" 在终端启动vim时，共享NERDTree
let g:nerdtree_tabs_open_on_console_startup=1
" 忽略一下文件的显示
let NERDTreeIgnore=['\.pyc','\~$','\.swp']
" 显示书签列表
let NERDTreeShowBookmarks=1
let NERDTreeWinPos=1

" vim不指定具体文件打开是，自动使用nerdtree
" autocmd StdinReadPre * let s:std_in=1
"autocmd VimEnter * if argc() == 0 && !exists("s:std_in") | NERDTree |endif
" 当vim打开一个目录时，nerdtree自动使用
 autocmd StdinReadPre * let s:std_in=1
 autocmd VimEnter * if argc() == 1 && isdirectory(argv()[0]) && !exists("s:std_in") | exe 'NERDTree' argv()[0] | wincmd p | ene | endif

" 寻找全局配置文件
let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/YouCompleteMe/cpp/ycm/.ycm_extra_conf.py'
" 禁用syntastic来对python检查
let g:syntastic_ignore_files=[".*\.py$"]
" 使用ctags生成的tags文件
let g:ycm_collect_identifiers_from_tag_files = 1
" 开启语义补全
" 修改对C语言的补全快捷键，默认是CTRL+space，修改为ALT+;未测出效果
"let g:ycm_key_invoke_completion = '<M-;>'
" 设置转到定义处的快捷键为ALT+G，未测出效果
"nmap <M-g> :YcmCompleter GoToDefinitionElseDeclaration <C-R>=expand("<cword>")<CR><CR>
"关键字补全
let g:ycm_seed_identifiers_with_syntax = 1
" 在接受补全后不分裂出一个窗口显示接受的项
set completeopt-=preview
" 让补全行为与一般的IDE一致
set completeopt=longest,menu
" 不显示开启vim时检查ycm_extra_conf文件的信息
let g:ycm_confirm_extra_conf=0
" 每次重新生成匹配项，禁止缓存匹配项
let g:ycm_cache_omnifunc=0
" 在注释中也可以补全
let g:ycm_complete_in_comments=1
" 输入第一个字符就开始补全
let g:ycm_min_num_of_chars_for_completion=1
" 错误标识符
let g:ycm_error_symbol='>>'
" 警告标识符
let g:ycm_warning_symbol='>*'
" 不查询ultisnips提供的代码模板补全，如果需要，设置成1即可
 let g:ycm_use_ultisnips_completer=1
"

"ale
"始终开启标志列
let g:ale_sign_column_always = 1
let g:ale_set_highlights = 0
"自定义error和warning图标
let g:ale_sign_error = '✗'
let g:ale_sign_warning = '⚡'
"在vim自带的状态栏中整合ale
let g:ale_statusline_format = ['✗ %d', '⚡ %d', '✔ OK']
"显示Linter名称,出错或警告等相关信息
let g:ale_echo_msg_error_str = 'E'
let g:ale_echo_msg_warning_str = 'W'
let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'
"普通模式下，sp前往上一个错误或警告，sn前往下一个错误或警告
nmap sp <Plug>(ale_previous_wrap)
nmap sn <Plug>(ale_next_wrap)

ab sp [<span id = " "></span>](#0)

set autoindent

"按下F9进行编译运行
map <F9> :call CompileRunGcc() <CR>
func! CompileRunGcc()
	silent exec "w"
	"	exec "!clear"
	"if &filetype == 'cpp'
	silent exec "!g++ %"
	exec "!./a.out"
	"elseif &filetype == 'java'
	"	exec "!javac %"
	"	exec "!java %<"
	"endif
endfunc
 
imap <c-]> {<cr>}<c-o>O<left><right>
	 
map <C-A> ggVG"+y
	 
	 
"inoremap ( ()<LEFT>
"inoremap [ []<LEFT>
"inoremap { {}<LEFT>
"inoremap " ""<LEFT>
"inoremap ' ''<LEFT>
	 	 
map <F2> :call SetTitle()<CR>
func SetTitle()
let l = 0

"let l = l + 1 | call setline(l,'/* ***********************************************')
"let l = l + 1 | call setline(l,'Author        :herongwei')
"let l = l + 1 | call setline(l,'Created Time  :'.strftime('%c'))
"let l = l + 1 | call setline(l,'File Name     :'.expand('%'))
"let l = l + 1 | call setline(l,'************************************************ */')
"let l = l + 1 | call setline(l,'')

"let l = l + 1 | call setline(l,'#include <cstdio>')
let l = l + 1 | call setline(l,'#include <cstring>')
let l = l + 1 | call setline(l,'#include <iostream>')
let l = l + 1 | call setline(l,'#include <algorithm>')
"let l = l + 1 | call setline(l,'#include <vector>')
"let l = l + 1 | call setline(l,'#include <queue>')
"let l = l + 1 | call setline(l,'#include <set>')
"let l = l + 1 | call setline(l,'#include <map>')
"let l = l + 1 | call setline(l,'#include <cmath>')
"let l = l + 1 | call setline(l,'#include <cstdlib>')
"let l = l + 1 | call setline(l,'#include <time.h>')
let l = l + 1 | call setline(l,'using namespace std;')
let l = l + 1 | call setline(l, '')
let l = l + 1 | call setline(l,'typedef long long ll;')
let l = l + 1 | call setline(l, '')
"let l = l + 1 | call setline(l,'const int maxn  = 1e5+233;')
"let l = l + 1 | call setline(l,'const int MOD   = 1e9+7;')
"let l = l + 1 | call setline(l,'const double eps= 1e-6;')
"let l = l + 1 | call setline(l,'const double pi = acos(-1.0);')
"let l = l + 1 | call setline(l,'int n,m,t,ans,res,tmp;  ')
let l = l + 1 | call setline(l,'int main(){')
"let l = l + 1 | call setline(l,'    //freopen("in.txt","r",stdin);')
"let l = l + 1 | call setline(l,'    //freopen("out.txt","w",stdout);')
let l = l + 1 | call setline(l,'    ')
let l = l + 1 | call setline(l,'    return 0;')
let l = l + 1 | call setline(l,'}')
endfunc

"空格键格式化当前行，并跳到下一行
imap <CR> <ESC> == o
