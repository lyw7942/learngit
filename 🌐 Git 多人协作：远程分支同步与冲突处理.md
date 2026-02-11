# **🌐 Git 多人协作：远程分支同步与冲突处理**

## **📌 这篇文章是干什么的？**

本文讲解 **团队使用 Git 协作时，如何正确推送、拉取远程分支，并处理同步冲突**。
核心包括：

- 远程仓库（`origin`）的基本概念
- 哪些分支需要推送，哪些可以本地保留
- 如何创建本地分支并关联远程分支
- 推送失败时的标准应对流程（`pull` → 合并 → `push`）

> 💡 Git 是分布式系统：**没有“中央服务器强制覆盖”，只有“协商合并”**。

------

## **🎯 使用场景**

- 多人同时开发 `dev` 分支
- 新成员加入项目，需同步远程所有分支
- 本地修改后推送被拒（因他人已更新）
- 首次在本地操作远程非主干分支（如 `feature`、`release`）

------

## **🛠️ 标准操作流程**

```shell
# 1. 克隆仓库（仅自动创建 main 分支）
git clone <repo-url>
cd repo

# 2. 创建本地 dev 并关联远程 origin/dev
git switch -c dev origin/dev

# 3. 正常开发 → 提交 → 推送
git add .
git commit -m "feat: xxx"
git push origin dev

# 4. 若推送失败（因远程有新提交）：
git pull                    # 拉取并自动合并

# 5. 若 pull 报错 “no tracking info”：
git branch --set-upstream-to=origin/dev dev
git pull

# 6. 若合并冲突：
#    - 手动编辑文件，解决 <<<<<<< ======== >>>>>>> 
#    - git add .
#    - git commit -m "fix: resolve conflict"

# 7. 再次推送
git push origin dev
```

------

## **🚀 日常工作流 · 极简指令汇总**

```shell
# 首次操作远程分支（如 dev）
git switch -c dev origin/dev

# 日常开发循环
git pull                    # 开始前先同步
# ... coding ...
git add .
git commit -m "message"
git push                    # 前提：已设置 upstream（首次用 -u）

# 首次推送时设置跟踪（推荐）
git push -u origin dev      # 之后 git push 即可
```

> ✅ 分支推送策略：
>
> - `main` / `dev`：必须推送到远程，团队共享
> - `feature/xxx`、`bugfix/xxx`：仅当你需要协作或备份时才推送
> - 个人实验分支：可完全本地，不推送到远程

------

## **⚠️ 特殊情况处理**

```shell
# 查看远程仓库信息
git remote -v

# 手动建立本地分支与远程分支的跟踪关系
git branch --set-upstream-to=origin/<branch> <local-branch>

# 查看当前分支的跟踪状态
git branch -vv

# 强制覆盖远程（危险！仅限个人分支）
# git push --force origin dev   # ❌ 团队分支禁用！

# 如果 pull 后想放弃合并
git merge --abort
```

> 💡 提示：
>
> - `git switch -c dev origin/dev` 会自动设置 tracking，无需额外配置
> - 首次 `push` 时加 `-u`（即 `--set-upstream`），后续可省略 `origin dev`
> - **永远不要对共享分支（如 main/dev）使用 `--force`**，会破坏他人历史

------

> ✅ **记住口诀**：
> **“克隆只有 main，其他分支手动连；
> 推送若被拒绝，先 pull 再 push；
> 冲突不怕，解决提交就成功！”**