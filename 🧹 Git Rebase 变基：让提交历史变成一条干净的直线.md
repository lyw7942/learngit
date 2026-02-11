# **🧹 Git Rebase 变基：让提交历史变成一条干净的直线**

## **📌 这篇文章是干什么的？**

本文介绍 **如何使用 `git rebase` 将分叉的本地提交“整理”成线性历史**，避免因 `git pull` 自动生成的合并提交导致历史混乱。

> 💡 核心思想：
> **“不是把别人的代码和我的代码拼起来，而是把我的修改重新在别人最新代码上做一遍。”**

适用于：个人功能分支开发、希望保持提交历史整洁的场景。

------

## **🎯 使用场景**

- 你在本地有未推送的提交
- 远程分支已被他人更新
- 你不想产生无意义的“Merge branch...”合并提交
- 希望 `git log` 看起来像一条直线，便于追溯

> ✅ 重要前提：**这些提交尚未推送给他人**（仅限私有提交）

------

## **🛠️ 标准操作流程**

```shell
# 1. 开发中，你有本地提交（如 add comment, add author）
# 2. 尝试推送，失败（因远程有新提交）
git push origin main

# 3. 不用普通 pull，而是用 rebase 模式同步
git pull --rebase origin main

# 4. 如果 rebase 过程中出现冲突：
#    - 手动编辑冲突文件
#    - git add .
#    - git rebase --continue

# 5. 若想放弃 rebase：
#    git rebase --abort

# 6. 成功后，历史变直线，正常推送
git push origin main
```

> 🔍 效果对比：
>
> - **merge 方式**：`A → B → C` + `A → D` → 合并为 `M`（分叉）
> - **rebase 方式**：`A → D → B' → C'`（直线）

------

## **🚀 日常工作流 · 极简指令汇总**

```shell
# 推荐：始终用 --rebase 拉取更新（避免自动 merge）
git pull --rebase

# 或者配置全局默认行为（推荐）
git config --global pull.rebase true

# 开发 → 提交 → 同步 → 推送
git add .
git commit -m "feat: xxx"
git pull --rebase      # 保持历史线性
git push               # 顺利推送
```

> ✅ 首次设置后，每次 `git pull` 都会自动 rebase，不再产生多余合并提交。

------

## **⚠️ 特殊情况与注意事项**

```shell
# 手动触发 rebase（在已 pull merge 后补救）
git rebase

# 解决 rebase 冲突后继续
git add .
git rebase --continue

# 放弃 rebase，回到操作前状态
git rebase --abort

# 查看当前是否在 rebase 中
ls .git/rebase-apply/   # 存在即正在 rebase
```

> ❗ **绝对不要对已共享的公共分支使用 rebase！**
>
> - 比如：`main`、`dev` 等团队共用分支
> - 因为 rebase 会**改变 commit ID**，导致他人本地历史错乱
>
> ✅ 安全范围：**仅用于你自己的 feature 分支，且尚未推送到远程或仅你一人使用**

------

> ✅ **记住口诀**：
> **“本地提交未推送，rebase 整理很清爽；
> 公共分支莫乱动，merge 才是安全方。”**