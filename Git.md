### 常用命令

```shell
# 初始化仓库
git init
# 添加文件到仓库
git add 文件
# 提交已add文件
git commit -m "提交说明"
# 提交已修改文件
git commit -am "提交说明"
# 查看仓库状态
git status
# 查看文件修改
git diff
# 查看提交日志
git log
# 回退版本
git reset --hard 版本ID
#  查看历史命令
git reflog
# 撤销工作区修改
git checkout -- 文件
# 撤销暂存区修改
git reset HEAD 文件
# 删除文件
git rm 文件
```

![git01](image/git01.jpeg)

### 分支命令

```shell
# 查看分支
git branch
# 创建分支
git branch 分支名
# 切换分支
git checkout 分支名
# 创建并切换分支
git checkout -b 分支名
# 合并分支到当前分支
git merge 分支名
--no-ff # 禁用Fast forward
# 删除分支
git branch -d 分支名
# 强制删除分支
git branch -D 分支名
# 图形化查看合并历史
git log --graph
```

### 工作区命令

```shell
# 保存工作区
git stash
# 查看所有工作区
git stash list
# 恢复工作区
git stash apple
# 删除工作区
git stash drop
# 恢复工作区并删除
git stash pop
```

### 远程仓库命令

```shell
# 克隆仓库
git clone 仓库地址
# 克隆指定分支
git clone -b 分支名 仓库地址
# 查看远程仓库
git remote -v
# 推送分支
git push 远程仓库名 分支名
# 拉取分支
git checkout -b 远程仓库名/分支名
# 拉取最新提交
git pull
```

### 标签命令

```shell
# 创建标签
git tag 标签名
# 给指定版本创建标签
git tag 标签名 版本ID
# 创建带说明标签
git tag -a 标签名 -m "说明文字" 版本ID
# 查看所有标签
git tag
# 查看标签信息
git show 标签名
# 删除标签
git tag -d 标签名
# 推送指定标签到远程
git push 远程仓库名 标签名
# 推送所有标签到远程
git push 远程仓库名 --tags
# 删除远程标签
git push 远程仓库名 :refs/tags/标签名
```

### 配置用户名邮箱

```shell
# 本仓库
git config user.name
git config user.email
# 全局
git config --global user.name
git config --global user.email
```

