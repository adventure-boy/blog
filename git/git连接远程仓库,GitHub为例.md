# 远程仓库

## 1,创建公钥私钥

git bush 中输入 下面命令

```sh
ssh-keygen -t rsa -C "123admin@163.com"
```

会生成公钥私钥

![image-20220330154012366](C:\Users\zjy52\AppData\Roaming\Typora\typora-user-images\image-20220330154012366.png)

## 2,GitHub设置

![image-20220330154416378](C:\Users\zjy52\AppData\Roaming\Typora\typora-user-images\image-20220330154416378.png)

将公钥复制到GitHub的如图位置

## 3,创建自己的仓库

![image-20220330154824310](C:\Users\zjy52\AppData\Roaming\Typora\typora-user-images\image-20220330154824310.png)

创建好后可以通过图片中的命令关联远程仓库

## 4,关联远程仓库

```sh
$ git remote add origin git@github.com:adventure-boy/gitRepository.git
```

添加后，远程库的名字就是origin，这是Git默认的叫法，可以改名

因为GitHub的分支默认名为main 所以必须把本地master分支改名

```sh
git branch -M main
```



## 5,推送信息到远程仓库

```shell
git push -u origin main
```

![image-20220330163515913](C:\Users\zjy52\AppData\Roaming\Typora\typora-user-images\image-20220330163515913.png)

