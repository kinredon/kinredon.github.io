+++
title = "如何利用 org mode 写博客"
date = 2021-10-05
lastmod = 2021-10-05T15:00:08+08:00
tags = ["Hugo", "Blog", "org mode", "easy hugo", "ox-hugo"]
categories = ["org mode"]
draft = false
katex = true
+++

## 前言 {#前言}

阅读本文需要一定的 emacs 和 org mode 使用基础，并阅读 [使用 Hugo 搭建个人网站（博客、个人主页）并发布到 Github 上](https://kinredon.github.io/post/how-to-publish-personal-website-on-github/) 。

由于我经常使用 [org mode](https://orgmode.org/) 进行 gtd、项目管理、记录笔记，因此计划写文章也用 [org mode](https://orgmode.org/) 来实现。 [org mode](https://orgmode.org/) 通常基于编辑器 emac 使用，是一种类似于 markdown 的标记语言，通过简单的符号定义，得到格式化的文章效果，关于 [org mode](https://orgmode.org/) 的使用可以参考[Org-mode 简明手册](https://www.cnblogs.com/open%5Fsource/archive/2011/07/17/2108747.html)。 [org mode](https://orgmode.org/) 具有强大的文档导出功能，比如 pdf，markdown，html 等，如果你没有使用过 org mode，强烈建议学习使用。之前使用了 Hugo 搭建了个人博客，虽然 Hugo 原生支持 org mode，但是实际效果不是很理想，毕竟没有像 markdown 应用广泛。因此基本思路是用 org mode 写文章，然后转化为 markdown 文本，幸运的是 emacs 社区具有相应的支撑。

主要使用到两个工具： `easy-hugo` 和 `ox-hugo=， =easy-hugo` 用来管理 Hugo 的文章， `ox-hugo` 用来将 org mode 文章转化为 markdown 格式。

关于如何使用 Hugo 搭建文章，可以查看 [使用 Hugo 搭建个人网站（博客、个人主页）并发布到 Github 上](https://kinredon.github.io/post/how-to-publish-personal-website-on-github/)。


## easy-hugo 的安装与使用 {#easy-hugo-的安装与使用}

查看 easy hugo 官网[ emacs-easy-hugo](https://github.com/masasam/emacs-easy-hugo)，可以发现 easy hugo 可以实现在 emac 上管理 Hugo 文章，如下图所示：

{{< figure src="/ox-hugo/pngpaste_clipboard_file_20211005133605.png" caption="Figure 1: 图源：![](https://github.com/masasam/emacs-easy-hugo/blob/master/image/easy-hugo-mode.png)" >}}

安装 easy hugo 通过 `MELPA` ，即 `M-x package-install easy-hugo` 配置如下：

```emacs-lisp
(use-package easy-hugo
  :init
  (setq easy-hugo-basedir "~/Documents/sync/blogs/kinredon.github.io") ;; 网站本地文件根目录
  (setq easy-hugo-url "https://kinredon.github.io") ;; url 路径
  (setq easy-hugo-sshdomain "kinredon.github.io")
  (setq easy-hugo-previewtime "300")
  (setq easy-hugo-default-ext ".org")
  :bind ("C-c C-e" . easy-hugo))
```

利用 `C-x C-e` 执行命令，然后 `M-x easy-hugo` 进入博客管理界面。


## ox-hugo 的安装与配置 {#ox-hugo-的安装与配置}

ox-hugo 可以完美转换 org mode 到 markdown，使用 `MeLPA` 安装，即 `M-x package-install ox-hugo` ，配置如下：

```emacs-lisp
(use-package ox-hugo
  :init
  (setq org-hugo-base-dir "~/Documents/sync/blogs/kinredon.github.io") ;; 本地网站根目录
  (setq org-hugo-default-section-directory "post")
  :ensure t
  :after ox)
```

至此，我们的配置完成，可以愉快地使用 org mode 撰写文章，自动转换到 markdown 格式，利用 hugo 生成静态网站，推送到 GitHub pages 上，也可以将 markdown 格式的文件传到其他支持 markdown 文章导入的平台，如知乎，微信公众号。

接下来介绍一下我写文章的工作流。


## 工作流 {#工作流}

我将 org mode 写的文章与生成的 markdown 文章分别管理，即在网站根目录添加 `org` 目录：

```shell
cd kinredon.github.io
mkdir org
```

`org` 目录中存储所有使用 org mode 写的文章，转化后的 markdown 文件存储在 `./content/post` 目录下，利用 easy hugo 管理。由于我不想上传 org 文件，因此我在 `.gitignore` 中加入 `org` 让 git 忽略此文件目录下的内容。

工作流步骤如下：

1.  撰写 org mode 文章，并存储在 `org` 目录下

    用 org mode 写文章有两种方式，一种是所有文章存储在一个 org 文件里面，不同的 subtree 为一个文章，利用 org capture 新建博文，生成模板文件，然后利用 org refile 将文章保存在不同的位置，这种方式[这里](https://www.xianmin.org/post/ox-hugo/)有讲解。我采用的方式为一篇文章一个 org 文件， 我们使用 `snippet` 模板, 即添加 `snippet` 模板：

    ```emacs-lisp
    # -*- mode: snippet -*-
    # name: hugo
    # key: h
    # --
    #+HUGO_BASE_DIR: path-to/kinredon.github.io
    #+TITLE: $1
    #+DATE: `(format-time-string "%Y-%m-%d")`
    #+HUGO_AUTO_SET_LASTMOD: t
    #+HUGO_TAGS: $2
    #+HUGO_CATEGORIES: $3
    #+HUGO_DRAFT: false
    # #+HUGO_MENU: :menu "main" :parent "docs" :weight 3
    #+options: author:nil
    #+HUGO_CUSTOM_FRONT_MATTER: :katex true

    $0
    ```

    保存后，我们新建 org 文件后，输入 `h` 然后使用 `tab` 就可生成模板文件，如下：

    ```text
    #+HUGO_BASE_DIR: path-to/kinredon.github.io
    #+TITLE:
    #+DATE: 2021-10-05
    #+HUGO_AUTO_SET_LASTMOD: t
    #+HUGO_TAGS:
    #+HUGO_CATEGORIES:
    #+HUGO_DRAFT: false
    # #+HUGO_MENU: :menu "main" :parent "docs" :weight 3
    #+options: author:nil
    #+HUGO_CUSTOM_FRONT_MATTER: :katex true
    ```

2.  导出 markdown 文件， `C-c C-e H h` 导出

    {{< figure src="/ox-hugo/pngpaste_clipboard_file_20211005140343.png" >}}

3.  预览

    通过 `M-x easy-hugo` 打开 easy hugo，然后选中新生成的文章，然后按 `p` 即可预览生成的文章。

4.  部署

    利用 [使用 Hugo 搭建个人网站（博客、个人主页）并发布到 Github 上](https://kinredon.github.io/post/how-to-publish-personal-website-on-github/) 中的 `deploy.sh` 将文章推送到远程服务器。

    ```shell
    #!/bin/bash
    git add .
    git add org/img # 用来存储图像的目录，后续将会用到
    git commit -m "update article"
    git push
    ```

    至此，可以在远程访问个人网站啦。


## 其它 {#其它}

在写作过程中发现一些小问题，比如文章图片的管理，文献管理等，这里介绍一下我的解决方案，后续遇到新的问题，再持续更新。


### 图片管理 {#图片管理}

我将图片放在 `./org/img` 目录下，普通的图片直接放在该目录下，然后在 org 文件中引用即可，然而我们写文章时常常喜欢截图，每次截图后保存非常的麻烦，因此利用一个 elisp 小函数实现该功能：

```emacs-lisp
(defun my/file-paste ()
  (interactive)
  (let* ((org-fpath (buffer-file-name (window-buffer (minibuffer-selected-window))))
         ;; (dst_dpath (expand-file-name (concat (if org-fpath (file-name-base org-fpath) "~/Desktop/scratch") "_" "assets")))
         (dst_dpath "./img")
         (src_fpath (string-trim (shell-command-to-string "/Users/kinredon/Documents/Scripts/clipboard/clipboard_file.sh")))
         (src_name (file-name-base src_fpath))
         (src_ext (file-name-extension src_fpath))
         (dst_fpath (concat dst_dpath "/" src_name "_" (format-time-string "%Y%m%d%H%M%S") "." src_ext)))
    (when (not (file-exists-p dst_dpath))
      (make-directory dst_dpath))
    (if (file-exists-p src_fpath)
        (progn (copy-file src_fpath dst_fpath)
               (insert (concat "[[file:" (file-relative-name dst_fpath (file-name-directory org-fpath)) "]]")))
      (message src_fpath))))


(use-package org
  :ensure nil
  :mode ("\\.org\\'" . org-mode)
  :bind
  (
   ("C-x C-y" . my/file-paste)
   ))
```

在 org 文件中， `C-x C-y` 自动调用 `file-paste` 函数，将截图自动存储在 `img` 目录下，然后生成对应的引用。


### 文献应用 {#文献应用}

由于我喜欢写论文阅读笔记，常常需要应用文献，关于文献管理，我使用的 `org-ref` 统一管理在一个 `reference.bib` 文件里，但是发现 `org-ref` 对 `ox-hugo` 似乎不太支持，因此使用 `citeproc-org` 进行简单的配置即可正常到处参考文献，如下：

```emacs-lisp
(citeproc-org-setup);; 可以将其写入配置文件中，或执行
```

在相应位置中插入参考文献，调用 `M-x org-ref-helm-insert-cite-link` 即可，导出文章即可以看到参考文献成功导出。


## 参考 {#参考}

1.  [Emacs Org-mode学术写作](https://zhuanlan.zhihu.com/p/54705090)
2.  [博客写作流程之工具篇： emacs, orgmode, hugo & ox-hugo](https://www.xianmin.org/post/ox-hugo/)
3.  [使用orgmode+hugo+github pages搭建博客](https://q3yi.me/post/build-blog-with-orgmode-hugo-and-github-pages/)
4.  [使用Emacs和Hugo academic主题](https://zlearning.netlify.app/linux/emacs/emacs-hugo-academic.html)
