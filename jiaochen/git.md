# git工具（以GitHub为例，Gitee操作基本相同）

↩️[返回主页]

## 1.两种方式在本地文件夹构建git环境(使用ssh连接)

具体ssh连接参照：[CSDN]

### 方式一：git clone下载Github远程仓库的完整项目，包含git环境

```sh
git clone git@github.com:xxxxx.git
```

### 方式二：在本地的文件夹中创建一个git环境（并把已有的工程同步到新建的github仓库）

Github新建一个仓库，不勾选add a readme file和add .gitignore

如果本地已有README，那么不需要执行echo "# This is a README" >> README

```sh
git init
echo "# This is a README" >> README.md
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:XXX/XXX.git
git push -u origin main
```

如上可以使得当前工作空间生成git环境，并将当前分支强制修改为main（因为github默认新建分支为main），然后通过-u使得当前分支与远程仓库的main绑定。后续在当前本地分支中，只需git push或者git pull即可，无需再指定origin main

~~注：_本地的各个分支（branch）可能与远程仓库branch不同名，数量也不同，所以git pull和git push指令都应该分别指定本地和远程branch。除非已经使用-u操作指定了本地的某个branch和远程的某个branch进行绑定_~~

~~例如，如果远程仓库存在A,B,C分支，本地仓库存在1，2分支，希望将远程的B和本地的1绑定，那么可以如下操作：~~

~~```sh~~
~~git push -u origin 1:B~~
~~```~~

~~之后，当位于本地的分支1时，只需git push或者git pull即可，因为已经与远程的分支B绑定~~

### 额外的: 本地工程已含有git环境

如果想把一个本地已含有git环境的仓库同步至一个新建的github中的远程仓库

```sh
git remote add origin git@github.com:XXX/XXX.git
git branch -M main
git push -u origin main
```

## 2.修改本地的代码

## 3.使用git管理提交版本和记录

```sh
git add .
git commit -m "提交描述"
```

## 4.将修改后的代码推送到远程仓库

```sh
git push
```

## 5.特殊操作

* 查看git状态

```sh
git status
```

* 查看当前remote关联的仓库

```sh
git remote -v
```

* 解绑remote关联的仓库

```sh
git remote remove <remote-name>
```

* 查看所有分支

```sh
git branch -a
```

* 查看当前分支

```sh
git branch
```

* 创建分支

```sh
git branch <new_branch>
```

* 切换分支

```sh
git checkout <branch_name>
```

* 创建并同时切换至新分支

```sh
git checkout -b <branch_name>
```

* 使用.gitignore文件来屏蔽不需要推送至远程仓库的文件

* 取消git已经跟踪的文件

```sh
git rm -r --cached back_end/node_modules
```

* 更改远程仓库的分支名称

```sh
git branch -r
git branch -m old_branch_name new_branch_name
git push origin :old_branch_name
git push origin new_branch_name
```

## 6.关于merge

例如main分支想要合并dev分支，切换到main分支

```sh
git merge dev
```

如果有冲突则解决冲突，并对所有冲突文件执行

```sh
git add <fileName>
```

全部解决后提交一次

```sh
git commit -m <content>
```

最后再次执行merge

```sh
git merge dev
```

本地完成合并后可以推送至远程仓库

```sh
git push
or
git push origin main
```

## 7.本地同时配置github和gitee

参照：[GIT系列（八）git同时配置gitee和github]

## 8.同时推送github和gitee

参照：[Git同时推送多个远程仓库]

[返回主页]:../README.md
[CSDN]:https://blog.csdn.net/weixin_42310154/article/details/118340458
[GIT系列（八）git同时配置gitee和github]:https://blog.csdn.net/qq_26849933/article/details/128292454?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2-128292454-blog-122931544.235%5Ev38%5Epc_relevant_sort_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7ERate-2-128292454-blog-122931544.235%5Ev38%5Epc_relevant_sort_base3&utm_relevant_index=5
[Git同时推送多个远程仓库]:https://zhuanlan.zhihu.com/p/141941373
