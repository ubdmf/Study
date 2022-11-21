# Git 常用语法

- `git push -f origin HEAD:user/xinyi/NT2ADR-907`  强推代码到某一指定分支
- `git add -A `(提交所有的改动)
- `git add . ` (提交当前目录的改动)
- ` git branch -M main  ` 新建一个主分支
- `git push -u origin/master` 第一次网远程分支推送代码   
- 修改以及查看用户信息 ![修改以及查看用户信息](./img/git_config_user_name.png)

## 如何新建一个本地仓库提交到远程
-  先在git创建一个仓库（不勾选创建readme.md ，直接creat，然后会有一些操作提示）
-  在本地新建一个文件 `mkdir study`
-  `code .` (在当前目录下打开vscode)
-  `control n` 创建一个文件夹，在第一行输入
-  `control shift ` ` 打开 vscode 内部的 terminal 
-  ` git init `
-  `git add —A ` (提交所有的改动)
- ` git commit -m "init" `
- `git push origin master ` 推到远程分支


