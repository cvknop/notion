https://wangchujiang.com/linux-command/c/git.html



#### 新建分支

```bash
git branch test # 新建test分支
git checkout -b xxx
# 将本地分支推到仓库
git push --set-upstream origin test
git branch --track dev origin/dev
```



#### stash

https://juejin.cn/post/7107973938591825933

```Bash
# 保存暂存和非暂存区文件
git stash 
git stash save "dev-uncommit-for?"

# 查看stash列表
git stash list

# 读取保存的状态，不抛出stash列表
git stash apply 0
# 读取保存的状态，抛出stash列表
git stash pop 1

移除单个修改：git stash drop <修改名>
清空所有修改：git stash clear
```

#### rebase

```Bash
# 查看所有分支的所有操作记录
git reflog --oneline 

# 将本地dev分支与远程dev分支之间建立链接
git branch --set-upstream dev origin/dev   
  
# 推送本地dev到远程，远程没有dev分支则自动创建 
git push --set-upstream origin dev
```