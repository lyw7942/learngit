# **🌿 Git 分支实战：安全开发新功能的标准流程**

## **📌 这篇文章是干什么的？**

本文介绍 **如何使用 Git 分支安全地开发新功能**，而不影响主干代码。
Git 的分支是轻量级指针，创建、切换、合并极快，**不会复制文件**，是团队协作和独立开发的核心工具。

通过一个标准工作流，你可以：

- 在隔离环境中实验新代码
- 保持 `main` 分支始终稳定可发布
- 完成后干净地合并并清理分支

> 💡 适用场景：添加功能、修复 bug、文档更新等任何独立任务。

------

## **🛠️ 标准操作流程（一键参考）**

```shell
# 1. 切换到主干（确保基于最新状态开发）
git switch main
git pull origin main          # 可选：拉取远程最新

# 2. 创建并切换到新分支（命名建议：feature/xxx, fix/xxx）
git switch -c feature/login

# 3. 开发你的功能 → 提交变更
#    （修改文件、新增文件等）
git add .
git commit -m "Add user login functionality"

# 4. 切回主干，合并成果
git switch main
git merge feature/login       # Fast-forward 合并

# 5. 清理分支并同步到远程
git branch -d feature/login   # 删除本地分支
git push origin main          # 推送更新到 GitHub
```

------

## **🚀 本文工作流 · 极简指令版（带注释）**

```shell
# 切回主干，确保从最新状态开始
git switch main

# 创建并切换到新功能分支
git switch -c feature/xxx

# 修改文件后，提交变更
git add .
git commit -m "Implement xxx"

# 切回主干，合并功能
git switch main
git merge feature/xxx

# 删除已合并的分支
git branch -d feature/xxx

# 推送主干更新到远程（需已设置 upstream）
git push
```

------

## **⚠️ 特殊情况处理**

```shell
# 强制删除未合并的分支（慎用！会丢失未合并的提交）
git branch -D feature/xxx

# 首次推送新分支到远程（建立跟踪关系）
git push -u origin feature/xxx

# 若合并时不是 Fast-forward（有分叉），创建合并提交
git merge --no-ff feature/xxx

# 查看分支是否已合并到当前分支
git branch --merged      # 已合并的分支
git branch --no-merged   # 未合并的分支
```

> 💡 提示：
>
> - `-d` 只能删**已合并**分支，安全；
> - `-D` = `--delete --force`，跳过检查，用于紧急清理。