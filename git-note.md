# 连接GitHub

#### 生成ssh密匙

需要写github账户对应的邮箱

```
ssh-keygen -t ed25519 -C "example@example.com"
```

将生成的`.pub`文件内容拷贝到 github的ssh key 中即可

具体可参考官方文档

https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

### Git常用命令

初始化仓库

```
git init
```

与远程仓库建立连接

```
git remote add origin git@explame.gihub.com:username/project/
```

查看所有分支(包括远程)

```
git branch -l-a
```

从远程仓库拉取一个分支到本地

```
git checkout -b xxxBranch origin/xxxBranch
```

将本地一个新分支推送到远程仓库

```
git push --set-upstream origin xxxBranch
```

切换分支

```
git checkout xxxBranch
```

暂存工作区

```
git stash
```



从B分支合并到A分支

```
# 确保当前分支为B分支
git merge A
```



pull = fetch + merge

重新设定

git pull --rebase

重新返回

git reset  



git rebase containue



vscode 插件

git graph

git lens