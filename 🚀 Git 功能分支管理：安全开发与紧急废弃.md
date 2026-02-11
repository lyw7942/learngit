# **🚀 Git 功能分支管理：安全开发与紧急废弃**

## **📌 这篇文章是干什么的？**

本文强调 **为每个新功能创建独立分支（feature branch）** 的最佳实践，并演示当功能被取消时，如何**安全地强制删除未合并的分支**。

核心原则：

- 功能开发必须隔离，避免污染主干或共享分支（如 `dev`）
- 即使功能废弃，也能通过 `-D` 安全清理，不留痕迹

> 💡 分支是“廉价实验场”——大胆开，放心删！

------

## **🎯 使用场景**

- 开发新功能（如 “Vulcan 引擎”）
- 实验性原型、技术调研
- 临时需求后被取消（如预算砍掉、需求变更）
- 需要彻底清理未合入主干的代码

------

## **🛠️ 标准操作流程**

```shell
# 1. 创建功能分支
git switch -c feature-vulcan

# 2. 开发并提交
# （编写代码）
git add .
git commit -m "add feature vulcan"

# 3. （正常情况）合并到 dev
# git switch dev
# git merge --no-ff -m "feat: vulcan" feature-vulcan
# git branch -d feature-vulcan

# 4. （异常情况）功能取消 → 强制删除
git branch -D feature-vulcan
```

> ✅ `-d`：仅允许删除**已合并**分支（安全）
> ✅ `-D`：强制删除**任何分支**（慎用，但必要时可靠）

------

## **🚀 日常工作流 · 极简指令汇总**

```shell
# 开发新功能
git switch -c feature/xxx
# ... coding ...
git add .
git commit -m "feat: xxx"

# 正常完成 → 合并后删除
git switch dev
git merge --no-ff -m "feat: xxx" feature/xxx
git branch -d feature/xxx

# 功能取消 → 直接强制删除
git branch -D feature/xxx
```

> ✅ 命名建议：使用 `feature/` 前缀，如 `feature/vulcan`

------

## **⚠️ 特殊情况处理**

```shell
# 尝试安全删除（仅当已合并）
git branch -d feature/xxx

# 强制删除未合并分支（会丢失未合入的提交！）
git branch -D feature/xxx

# 查看哪些分支尚未合并到当前分支
git branch --no-merged

# 确认分支内容后再删除（可选）
git log feature/xxx --oneline -5
```

> 💡 提示：
>
> - `-D` 是 `--delete --force` 的缩写
> - 删除分支**不会删除提交历史**（只要其他分支引用了这些提交）
> - 但如果该分支是**唯一引用**，其提交可能在未来被 Git 垃圾回收（gc）清除
> - 如需保留代码备份，可先 `git tag save-vulcan feature-vulcan` 再删除分支

------

> ✅ **记住口诀**：
> **“功能开分支，完成再合并；
> 若被中途砍，-D 直接删。”**