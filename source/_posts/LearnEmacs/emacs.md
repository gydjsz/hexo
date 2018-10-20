---
title: emacs配置
---
(require 'package)
(package-initialize)
(add-to-list'package-archives '("melpa" . "http://melpa.milkbox.net/packages/") t)
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(custom-safe-themes
   (quote
    ("8aebf25556399b58091e533e455dd50a6a9cba958cc4ebb0aab175863c25b9a4" "bffa9739ce0752a37d9b1eee78fc00ba159748f50dc328af4be661484848e476" default)))
 '(display-time-mode t)
 '(package-archives
   (quote
    (("gnu" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/")
     ("melpa" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/"))))
 '(package-selected-packages
   (quote
    (emms emms-bilibili w3m eimp dracula-theme ecb yasnippet-snippets sr-speedbar auto-yasnippet yasnippet-classic-snippets yasnippet electric-spacing spacemacs-theme solarized-theme flymake-cppcheck flycheck-title flycheck-objc-clang flycheck-irony flycheck-color-mode-line flycheck auto-complete-c-headers auto-complete)))
 '(show-paren-mode t))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(default ((t (:family "Ubuntu Mono" :foundry "DAMA" :slant normal :weight normal :height 158 :width normal)))))

(global-auto-complete-mode t)

;(global-linum-mode 1) ; always show line numbers                              
;(setq linum-format "%d)")  ;set format
(global-linum-mode t)

(global-flycheck-mode t)


;;启用时间显示设置，在minibuffer上面的那个杠上
(display-time-mode 1)
;;时间使用24小时制
(setq display-time-24hr-format t)
;;时间显示包括日期和具体时间
;(setq display-time t)

;; TAB键的宽度设置为4
(setq c-basic-offset 4)

;(load-theme 'spacemacs-dark t)  ;设置Spacemacs-dark主题

(load-theme 'dracula t)

;(global-hl-line-mode 1)   ;设置行号

;;让Emacs可以直接打开和显示图片。
(setq auto-image-file-mode t)


(global-set-key (kbd ",")   ;逗号后面自动添加空格
                #'(lambda ()
                    (interactive)
                    (insert ", ")))


;; 以 y/n代表 yes/no
(fset 'yes-or-no-p 'y-or-n-p) 

;; 语法高亮
(global-font-lock-mode t)

;; 显示括号匹配 
(show-paren-mode t)
(setq show-paren-style 'parentheses)

;; 支持emacs和外部程序的粘贴
(setq x-select-enable-clipboard t)

;; 在标题栏提示你目前在什么位置
(setq frame-title-format "zhj@%b") 

;; 回车缩进
(global-set-key "\C-m" 'newline-and-indent)
(global-set-key (kbd "C-<return>") 'newline)

;(yas-global-mode 1) 

(add-to-list 'load-path "~/.emacs.d/plugins/yasnippet")
(require 'yasnippet)
(setq yas/prompt-functions 
   '(yas/dropdown-prompt yas/x-prompt yas/completing-prompt yas/ido-prompt yas/no-prompt))
(yas/global-mode 1)
(yas/minor-mode-on) ; 以minor mode打开，这样才能配合主mode使用

(require 'auto-yasnippet)                                                           
(global-set-key (kbd "H-w") #'aya-create)                                           
(global-set-key (kbd "H-y") #'aya-expand)   



;(sr-speedbar-open)

(auto-image-file-mode)

;;关闭emacs启动时的画面
(setq inhibit-startup-message t)

;; 改变 Emacs 固执的要你回答 yes 的行为。按 y 或空格键表示 yes，n 表示 no
(fset 'yes-or-no-p 'y-or-n-p)

;;设置emacs GUI下的字体
(set-default-font "-bitstream-Courier 10 Pitch-normal-normal-normal-*-29-*-*-*-m-0-iso10646-1")

(add-to-list 'load-path "/home/dal/.emacs.d/elpa/ecb-20170728.1221")


;;;; 各窗口间切换

(global-set-key [M-left] 'windmove-left)

(global-set-key [M-right] 'windmove-right)

(global-set-key [M-up] 'windmove-up)

(global-set-key [M-down] 'windmove-down)




;; Disable buckets so that history buffer can display more entries
;(setq ecb-history-make-buckets 'never)

(defun set-image-mode-mwheel-scroll-function ()
  (setq-local mwheel-scroll-down-function 'image-scroll-down)
  (setq-local mwheel-scroll-up-function 'image-scroll-up))

(add-hook 'image-mode-hook #'set-image-mode-mwheel-scroll-function)

;;emacs启动的时候自动全屏
(add-to-list 'default-frame-alist '(fullscreen . maximized))

;(require 'sr-speedbar)

//隐藏工具栏, 菜单栏, 滚动条
;(tool-bar-mode 0) 
;(menu-bar-mode 0) 
;(scroll-bar-mode 0) 


(global-auto-revert-mode t)

;(add-hook 'c-mode-common-hook ( lambda() ( c-set-style "k&r" ) ) ) ;;设置C语言默认格式
(add-hook 'c++-mode-common-hook ( lambda() ( c-set-style "k&r" ) ) ) ;;设置C++语言默认格式

;(global-set-key [(f8)] 'loop-alpha)  ;;注意这行中的F8 , 可以改成你想要的按键    
    
(setq alpha-list '((85 55) (100 100)))    
    
(defun loop-alpha ()    
  (interactive)    
  (let ((h (car alpha-list)))                    
    ((lambda (a ab)    
       (set-frame-parameter (selected-frame) 'alpha (list a ab))    
       (add-to-list 'default-frame-alist (cons 'alpha (list a ab)))    
       ) (car h) (car (cdr h)))    
    (setq alpha-list (cdr (append alpha-list (list h))))    
    )    
)    
  
  
;打开emacs 之后 按F8 可以在透明 和不透明之间切换...   

(setq is-alpha nil) 

(defun transform-window (a ab) 
  (set-frame-parameter (selected-frame) 'alpha (list a ab)) 
  (add-to-list 'default-frame-alist (cons 'alpha (list a ab))) 
) 

(global-set-key [(f8)] (lambda() 
                         (interactive) 
                         (if is-alpha 
                             (transform-window 100 100) 
                           (transform-window 85 50)) 
                         (setq is-alpha (not is-alpha)))) 




;; Enable EDE (Project Management) features
(global-ede-mode 1)


(custom-set-variables
'(semantic-default-submodes (quote (global-semantic-decoration-mode global-semantic-idle-completions-mode
global-semantic-idle-scheduler-mode global-semanticdb-minor-mode
global-semantic-idle-summary-mode global-semantic-mru-bookmark-mode)))
'(semantic-idle-scheduler-idle-time 3))

(semantic-mode)

;; smart complitions
(require 'semantic/ia)
(setq-mode-local c-mode semanticdb-find-default-throttle
'(project unloaded system recursive))
(setq-mode-local c++-mode semanticdb-find-default-throttle
'(project unloaded system recursive))

;;;; TAGS Menu
(defun my-semantic-hook ()
(imenu-add-to-menubar "TAGS"))

(add-hook 'semantic-init-hooks 'my-semantic-hook)

;;;; Semantic DataBase存储位置
(setq semanticdb-default-save-directory
(expand-file-name "~/.emacs.d/semanticdb"))

;; 使用 gnu global 的TAGS。
(require 'semantic/db-global)
(semanticdb-enable-gnu-global-databases 'c-mode)
(semanticdb-enable-gnu-global-databases 'c++-mode)

;;;; 缩进或者补齐
;;; hippie-try-expand settings
(setq hippie-expand-try-functions-list
'(
yas/hippie-try-expand
semantic-ia-complete-symbol
try-expand-dabbrev
try-expand-dabbrev-visible
try-expand-dabbrev-all-buffers
try-expand-dabbrev-from-kill
try-complete-file-name-partially
try-complete-file-name
try-expand-all-abbrevs))

(defun indent-or-complete ()
"Complete if point is at end of a word, otherwise indent line."
(interactive)
(if (looking-at "\\>")
(hippie-expand nil)
(indent-for-tab-command)
))

(defun yyc/indent-key-setup ()
;"Set tab as key for indent-or-complete"
(local-set-key [(tab)] 'indent-or-complete)
)

(require 'cedet)

(semantic-mode 1)

(global-ede-mode 1)

(global-set-key (kbd "<f5>") 'sr-speedbar-toggle) ;;sr-speedbar按键绑定


;;;; 自动启动ecb，并且不显示每日提示

(setq ecb-auto-activate t

     ecb-tip-of-the-day nil)
