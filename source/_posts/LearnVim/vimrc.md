---
title: vim配置
---
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

" 设置NerdTree
map <F5> :NERDTreeMirror<CR>
map <F5> :NERDTreeToggle<CR>

colorscheme slate

call pathogen#infect()
syntax on
filetype plugin indent on

" TagBar 自动生成参数和方法
" Then the F8 key will toggle the Tagbar window.
map <F8> :TagbarToggle<CR>  
map <C-c> :TagbarToggle<CR> 
			
"syntastic 保存检查代码时候传入参数
let g:syntastic_java_javac_args="-cp ../../lib:../../bin -sourcepath ../../bin -Djava.ext.dirs=../../lib -d ../../bin"

"vim-commentary
"块模式：v 命令选中需要注释的内容，gc 注释，取消注释也是同样的步骤。
"V 命令，选中当前行，gc 注释当前正行内容，这个使用是最方便的，也是最多的。

"bufexplorer
"命令模式：\be 跳转到缓冲区窗口，然后 选择需要的缓冲区回车
 
