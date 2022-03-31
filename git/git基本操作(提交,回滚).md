# git配置及基本操作(提交,回滚)

## 1,Git配置

Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。

- `etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。

- `~/.gitconfig` 文件：用户目录下的配置文件只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。

- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

  可通过下面命令修改配置

  

```shell
git config --system --edit;
git config --global --edit
```

## 2,用户信息配置

```
git config --global user.name "zhangsan"
git config --global user.email 123admin@163.com
```

## 3,创建版本库

![image-20220330115404873](C:\Users\zjy52\AppData\Roaming\Typora\typora-user-images\image-20220330115404873.png)

使用Git Bash 命令行工具

cd 命令进入文件夹,mkdir创建文件夹,

## 4,初始化目录

通过git init命令把这个目录变成Git可以管理的仓库,创建成功后在该文件夹下会多出一个.git的文件夹。

## 5,基本操作

### git add

1. 在工作目录下创建hello.txt文件,并输入内容
2. 通过git add命令把文件添加到git仓库

```shell
git add hello.txt
```

### git commit

git commit可将文件提交到仓库, -m 后面接备注信息

```shell
$ git commit hello.txt -m "third commit"
[master d17f72d] third commit
 1 file changed, 1 insertion(+), 1 deletion(-)
```

### git add 和git commit 的区别

- 工作区

当你在开发一个项目时，主目录就是你的工作区。

- 暂存区

Git的版本库里存了很多文件，其中包括称为Stage或index的暂存区，还有一个git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针`HEAD`

- 版本库

工作区中有一个隐藏目录`.git`，这个就是git的版本库了。

下图为整个git的流程图:

![](D:\Note\git_procedure_v2.png)

- #### 区别

`git add`和`git commit`的区别就在于：
`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；
`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。
因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支。所以，git commit就是往master分支上提交更改。
你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

所以要想将修改提交到master中一定要先`git add`到暂存区中，再`git commit`到master分支。

### status和diff命令

| 命令       | 说明                                                      |
| ---------- | --------------------------------------------------------- |
| git status | 查看当前库的状态                                          |
| git diff   | 是difference的简写是用来查看文件的变化的 (工作区和版本库) |

```sh
$ git add hello.txt

zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (master)
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   hello.txt

```

修改hello.txt文件,使用git diff命令

```sh
$ git diff hello.txt
diff --git a/hello.txt b/hello.txt
index 730c884..288951a 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1 @@
-hello,world!!!!  你好!!!!!!!!!
\ No newline at end of file
+hello,world!!!!  你好!!!!!!!!! 查看git diff命令
\ No newline at end of file

```

### 版本回退

- log命令:

git log命令显示从最近到最远的提交日志,每条日志信息占了五行记录，如果日志比较多的情况下。我们可以在命令后添加 –pretty=oneline单行来显示日志信息.其中commit 后面的字符串为版本号

```sh
$ git log
commit 931328a112a43eb7f7ea095a636ee24134028d26 (HEAD -> master)
Author: adventure-boy <zjy521995@163.com>
Date:   Wed Mar 30 14:34:04 2022 +0800

    git diff order

commit d17f72d0599d4f4ac0d31acc0d06576242ace2c4
Author: adventure-boy <zjy521995@163.com>
Date:   Wed Mar 30 14:17:21 2022 +0800

    third commit

commit 1ba454a1752b38e1ed6dfb7dabe004166a2a50f0
Author: adventure-boy <zjy521995@163.com>
Date:   Wed Mar 30 11:04:29 2022 +0800

    second commit

commit bf2e44745b52ff1b374ee55a45e2766765f5584a
Author: adventure-boy <zjy521995@163.com>
Date:   Wed Mar 30 11:00:47 2022 +0800

    frist commit


```

- git reset --hard HEAD^    回退到上一个版本
- git reset --hard 版本号   回退到指定版本

- git reflog  可查看所有的命令信息,以及版本号

  ```sh
  $ git reflog
  d17f72d (HEAD -> master) HEAD@{0}: reset: moving to HEAD^
  931328a HEAD@{1}: commit: git diff order
  d17f72d (HEAD -> master) HEAD@{2}: commit: third commit
  1ba454a HEAD@{3}: reset: moving to 1ba454a
  bf2e447 HEAD@{4}: reset: moving to HEAD^
  4ebabaa HEAD@{5}: reset: moving to HEAD
  4ebabaa HEAD@{6}: reset: moving to HEAD
  4ebabaa HEAD@{7}: commit: second change
  bf2e447 HEAD@{8}: reset: moving to bf2e44745b52ff1b374ee55a45e2766765f5584a
  1ba454a HEAD@{9}: commit: second commit
  bf2e447 HEAD@{10}: commit (initial): frist commit
  
  ```

  ### 撤销

  - git checkout -- file  命令:当文件发生改变但是没执行git add命令时,使用此命令可以撤销文件的修改
  - git reset HEAD file 命令:当文件git add提交到暂存区时,可以使用此命令使文件回退到工作区
  - git reset --hard HEAD^ 命令:当文件git commit 提交到版本库而为提交到远程仓库时,,可以使用此命令回退到上一个版本
  - git reset --hard 版本号  命令:当文件git commit 提交到版本库而为提交到远程仓库时,,可以使用此命令 回退到指定版本

  
