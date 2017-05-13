---
layout: post
title: Emacs配置及插件
date: 2015-02-08
category: Edit
tag: Emacs
---


**摘要:**
有人说emacs是神的编辑器,很是羡慕那些大神用这么牛逼的编辑器,还可以配置成牛逼的ide
都无法直视那些用emacs的开发者,给你们点赞;我以前的一个公司CEO用的就是emacs写代码;
这里是我用过的配置文件和快捷键内容,保留在这里,以供查阅.



#### Emacs Config

```
;;打开emacs 交互解释器
;;M+x ielm
(let ((default-directory "~/.emacs.d/el-get/"))
  (normal-top-level-add-subdirs-to-load-path))

(add-to-list 'load-path "~/.emacs.d/el-get/yasnippet/")
(add-to-list 'load-path "~/.emacs.d/el-get/rope/")
(add-to-list 'load-path "~/.emacs.d/el-get/pymacs/")
(add-to-list 'load-path "~/.emacs.d/el-get/ropemacs/")
(add-to-list 'load-path "~/.emacs.d/el-get/ropemode/")
(add-to-list 'load-path "~/.emacs.d/el-get/auto-complete/")
(add-to-list 'load-path "~/.emacs.d/el-get/virtualenvwrapper/")
(add-to-list 'load-path "~/.emacs.d/el-get/virtualenv.el-master/")
(add-to-list 'load-path "~/.emacs.d/el-get/python-django/")

(load "~/.emacs.d/el-get/el-get/el-get.el")
(load "~/.emacs.d/el-get/python-django/python-django.el")
(load "~/.emacs.d/el-get/virtualenv.el-master/virtualenv.el")
(load "~/.emacs.d/el-get/auto-complete/auto-complete.el")

(tool-bar-mode 0) 
(menu-bar-mode 0) 
(scroll-bar-mode 0)
(custom-set-variables
 ;; custom-set-variables was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 '(virtualenv-root "~/workspace/taobao-backend/shopmanager/ve/lib/python2.7/"))
(custom-set-faces
 ;; custom-set-faces was added by Custom.
 ;; If you edit it by hand, you could mess it up, so be careful.
 ;; Your init file should contain only one such instance.
 ;; If there is more than one, they won't work right.
 )

;;关闭启动页面
(setq inhibit-startup-message t)
;;设置备份保存路径
(setq backup-directory-alist (quote (("." . "~/.backups"))))

;;配置virtualenv

(setenv "PATH" (concat (getenv "PATH") ":" "~/path to you virtual env example: /ve/bin"))
(add-to-list 'exec-path "~/path to you virtual env example: /ve/bin")

;;speedbar 配置 下载的el文件在el-get/sr-speedbar
(require 'sr-speedbar)

;; 配置显示行号
(global-linum-mode t)
;;窗口切换
(global-set-key (kbd "C-S-j") 'windmove-down)
(global-set-key (kbd "C-S-k") 'windmove-up)
(global-set-key (kbd "C-S-h") 'windmove-left)
(global-set-key (kbd "C-S-l") 'windmove-right)
```

---

#### emacs virtualenv config

[Referance Link](http://stackoverflow.com/questions/17759610/use-el-get-to-install-jedi-for-emacs-on-mac)


---

#### webkit in emacs

M+x

el-get-install webkit

sudo apt-get install qt5-default

---

#### Emacs Short Cuts


| category	|cmd		|explain |
|---------------|---------------|---------|
|打开  		|emacs -nw	|命令行打开emacs
|文件操作	|C-x C-f	|打开文件,出现提示时输入/username@host:filepath可编辑FTP文件
|	|C-x C-v 	|打开一个文件，取代当前缓冲区
|	|C-x C-s 	|保存文件
|	|C-x C-w 	|存为新文件
|	|C-x i		|插入文件
|	|C-x C-q	|切换为只读或者读写模式
|	|C-x C-c 	|退出Emacs
|编辑操作|C-f	|前进一个字符
|	|C-b 	|后退一个字符
|	|M-f 	|前进一个字
|	|M-b 	|后退一个字
|	|C-a 	|移到行首
|	|C-e 	|移到行尾
|	|M-a	|移到句首
|	|M-e 	|移到句尾
|	|C-p 	|后退一行
|	|C-n	|前进一行
|	|M-x goto-line	|跳到指定行
|	|C-v 		|向下翻页
|	|M-v 		|向上翻页
|	|M-< 		|缓冲区头部
|	|M-> 		|缓冲区尾部
|	|C-M-f 		|向前匹配括号
|	|C-M-b 		|向后匹配括号
|	|C-l 		|当前行居中
|	|M-n or C-u n 	|重复操作随后的命令n次
|	|C-u		|重复操作随后的命令4次
|	|C-u C-u	|重复操作随后的命令8次
|	|C-x ESC ESC	|执行历史命令记录，M-p选择上一条命令，M-n选择下一条命令
|	|C-d 	|删除一个字符
|	|M-d 	|删除一个字
|	|C-k 	|删除一行
|	|M-k 	|删除一句
|	|C-w 	|删除标记区域
|	|C-y 	|粘贴删除的内容
|	|C-@	|标记开始区域
|	|C-x h 	|标记所有文字
|	|C-x C-x|交换光标位置和区域标记区开头
|	|M-w 	|复制标记区域
|	|C-_ or C-x u		|撤消操作
|	|M-x shell 		|打开SHELL	执行SHELL命令
|	|M-! 			|执行SHELL命令 (shell-command)
|	|M-1 M-!		|执行SHELL命令,命令输出插入光标位置,不打开新输出窗口
|	|C-x r t 		|输入注释内容	多行注释（光标起始列到光标结束列之间填充注释内容）
|	| c-x r k		|反多行注释(选中到最后一行紧挨着注释字符之后的位置按)
|	|M C-\ 	|自动缩进光标和标记间的区域
|	|M-m 	|移动光标到行首第一个(非空格)字符
|	|M-^ 	|将当前行接到上一行末尾处
|	|M-; 	|添加缩进并格式化的注释
|	|C-c C-q 	|根据缩进风格缩进整个函数
|	|C-c C-a 	|切换自动换行功能
|	|C-c C-d 	|一次性删除光标后的一串空格(greedy delete)
|	|M-| 	|针对某一特定区域执行命令(shell-command-on-region), 比如 C-x h M-|uuencode
|窗口操作	|C-x 0 	|关闭本窗口
|	|C-x 1 	|只留下一个窗口
|	|C-x 2 	|垂直均分窗口
|	|C-x 3 	|水平均分窗口
|	|C-x o 	|切换到别的窗口
|	|C-x s 	|保存所有窗口的缓冲
|	|C-x b 	|选择当前窗口的缓冲区
|	|C-x ^ 	|纵向扩大窗口
|	|C-x } 	|横向扩大窗口
|	|C-x {	|横向缩小窗口
|缓冲区列表操作	|C-x C-b 	|打开缓冲区列表
|	|d or k 		|标记为删除
|	|~ 	|标记为未修改状态
|	|%	|标记为只读
|	|s 	|保存缓冲
|	|u 	|取消标记
|	|x 	|执行标记的操作
|	|f 	|在当前窗口打开该缓冲区
|	|o 	|在其他窗口打开该缓冲区
|目录操作说明：	|目录模式中，如果输入!，在命令行中包含*或者?，有特殊的含义。*匹配当前光标所在的文件和所有标记的文件，?分别在每一个标记的文件上 执行该命令。
|		|C-x d    |打开目录模式
|	|s 	|按日期/文件名排序显示
|	|v 	|阅读光标所在的文件
|	|q 	|退出阅读的文件
|	|d 	|标记为删除
|	|x 	|执行标记
|	|D 	|马上删除当前文件
|	|C 	|拷贝当前文件
|	|R 	|重名名当前文件
|	|＋	|新建文件夹
|	|Z 	|压缩文件
|	|! 	|对光标所在的文件执行SHELL命令
|	|g 	|刷新显示
|	|i 	|在当前缓冲区的末尾插入子目录的内容
|	|[n]m 	|标记光标所在的文件，如果指定n，则从光标所在的文件起后n个文件被标记
|	|[n]u 	|取消当前光标标记的文件，n的含义同上
|	|t 	|反向标记文件
|	|%-m 	|正则标记
|	|q 	|退出目录模式
|程序编译|	|M-x compile 	|执行编译操作
|	|M-x gdb |	GDB排错
|	|M-x dbx |	DBX排错
|	|M-x xdb |	XDB排错
|	|M-x sdb |	SDB排错
|搜索模式	|C-s key	|向前搜索
|	|C-s 	     		|查找下一个
|	|ENTER 			|停止搜索
|	|C-r key 		|反向搜索
|	|C-s C-w 		|以光标所在位置的字为关键字搜索
|	|C-s C-s 		|重复上次搜索
|	|C-r C-r 		|重复上次反向搜索
|	|C-s ENTER C-w 		|进入单词搜索模式
|	|C-r ENTER C-w 		|进入反向单词搜索模式
|	|M-x replace-string ENTER search-string ENTER 	|替换(选中的)不选中则从光标位置替换到结尾
|	|M-% search-string ENTER replace-string ENTER 	|交互替换（空格则跳过y则替换，无需选中）
|	|C-r 		   |在进入单词搜索(查找/替换)模式后，该命令进入迭代编辑模式
|	|C-M-x		   |退出迭代编辑模式，返回到查找/替换模式
|	|C-M- s 	   |向前正则搜索
|	|C-M-r 		   |向后正则搜索
|	|M-,(逗号) 	   |在tags-search里跳至下一个匹配处
|	|M-x tags-query-replace		|在设置过标签的所有文件里替换文本
|	|M-x tags-search ENTER 		|在所有标签里搜索(使用正则表达式)
|	|M-.(点) 	       		|搜索标签
|	|C-M-% 				|正则交互替换
|shell	|C-c C-c 			|相当于Bash下的C-c
|	|C-c C-z 			|相当于Bash下的C-z
|	|C-c C-d 			|相当于Bash下的C-d
|	|M-p 				|执行前一条命令
|	|C-n 				|执行下一条命令
|	|C-c C-o 			|删除最后一条命令产生的输出
|	|C-c C-r 			|屏幕滚动到最后一条命令输出的开头
|	|C-c C-e			|屏幕滚动到最后一套命令输出的结尾
|	|C-c C-p 			|查看前一条命令的输出
|	|C-c C-n 			|查看后一条命令的输出
|打印资料    	|M-x print-buffer	|先使用pr,然后使用lpr
|		|M-x lpr-buffer			|直接使用lpr
|		|M-x print-region
|		|M-x lpr-region
|收发邮件	|M-x mail			|发送邮件, C-c C-s 发送,C-c C-c 发送并退出
|		|M-x rmail 			|接受邮件
|版本控制	|C-x v d 			|显示当前目录下所有注册过的文件(show all registered files in this dir)
|	|C-x v = 	|比较不同版本间的差异(show diff between versions)
|	|C-x v u 	|移除上次提交之后的更改(remove all changes since last checkin)
|	|C-x v ~ 	|在不同窗格中显示某个版本(show certain version in different window)
|	|C-x v l 	|打印日志(print log)
|	|C-x v i 	|标记文件等待添加版本控制(mark file for version control add)
|	|C-x v h 	|给文件添加版本控制文件头(insert version control header into file)
|	|C-x v r 	|获取命名过的快照(check out named snapshot)
|	|C-x v s 	|创建命名的快照(create named snapshot)
|	|C-x v a 	|创建gnu风格的更改日志(create changelog file in gnu-style)





