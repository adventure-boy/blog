

# 分支管理

所谓的分支管理其实就是就是同时可以有多条时间线在执行，最终合并为一个点，有点类似于多线程操作，这也正是git有别于其他版本控制软件的地方。

![](D:\Note\git-branches-merge.png)



## 查看当前分支

```sh
git branch
```

## 创建新的分支

```sh
git branch dev1
```

## 切换分支

```sh
git checkout dev1
```

## 创建并切换分支

```sh
git checkout -b dev2
```

## 合并分支

```sh
git merge dev2
```



案例:

- dev1分支修改文件

```sh
$ cat hello.txt
dev1分支
hello,world
dev1 add
```

- dev1分支git add ,git commit 

```sh
zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (dev1)
$ git commit hello.txt -m "dev1 commit"
warning: LF will be replaced by CRLF in hello.txt.
The file will have its original line endings in your working directory
[dev1 038fe3c] dev1 commit
 1 file changed, 3 insertions(+), 1 deletion(-)
```

- 在main分支是不能看到修改内容的

```sh
zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
$ cat hello.txt
hello,world!!!!  你好
```

- 合并分支

  ```sh
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ git merge dev1
  Updating d17f72d..038fe3c
  Fast-forward
   hello.txt | 4 +++-
   1 file changed, 3 insertions(+), 1 deletion(-)
  
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ cat hello.txt
  dev1分支
  hello,world
  dev1 add
  
  ```

  ## 删除分支

  ```sh
  git branch -d dev1
  ```

  ## 解决冲突

  当不同分支修改内容出现冲突时解决方案

  案例:

  - main分支修改内容

  ```sh
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ vi hello.txt
  
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ cat hello.txt
  dev1分支
  hello,world
  main add
  ```

  - 修改dev2分支内容

  ```sh
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ git merge dev2
  Auto-merging hello.txt
  CONFLICT (content): Merge conflict in hello.txt
  Automatic merge failed; fix conflicts and then commit the result.
  ```

  

  - 合并分支

  ```sh
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ git merge dev2
  Auto-merging hello.txt
  CONFLICT (content): Merge conflict in hello.txt
  Automatic merge failed; fix conflicts and then commit the result.
  
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main|MERGING)
  $ cat hello.txt
  dev1分支
  hello,world
  <<<<<<< HEAD   			主分支
  main add
  =======					分界线
  dev2 add
  >>>>>>> dev2			dev2分支
  ```

  Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容<<<HEAD是指主分支修改的内容，>>>>>dev2是指dev2上修改的内容
  直接修改

  - 解决冲突

  ```sh
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main|MERGING)
  $ vi hello.txt
  
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main|MERGING)
  $ git add hello.txt
  
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main|MERGING)
  $ git commit -m "merging commit"
  [main 491f5ce] merging commit
  
  zjy52@DESKTOP-F7HO65P MINGW64 /d/git/gitRepository (main)
  $ git merge dev2
  Already up to date.
  ```

  - 查看合并情况

```
git log --graph --pretty=oneline --abbrev-commit
```

```sh
$ git log --graph --pretty=oneline --abbrev-commit
*   491f5ce (HEAD -> main) merging commit
|\
| * 2803f44 (dev2) dev2 commit    合并情况
* | 3e1b1b3 main commit
|/
* 038fe3c (origin/main, dev1) dev1 commit
* d17f72d third commit
* 1ba454a second commit
* bf2e447 frist commit
```

