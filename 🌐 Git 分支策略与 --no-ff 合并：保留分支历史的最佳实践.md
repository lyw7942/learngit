# **🌐 Git 分支策略与** `--no-ff` **合并：保留分支历史的最佳实践**

## **📌 这篇文章是干什么的？**

本文介绍 **如何通过 `git merge --no-ff` 强制创建合并提交（merge commit）**，从而在 Git 历史中**显式保留分支存在和合并记录**。
同时提出一套**团队协作的分支管理策略**：

- `main`（或 `master`）只用于**稳定发布**
- 所有开发工作在 `dev` 分支或个人功能分支上进行

> 💡 默认的 Fast-forward 合并虽然高效，但会“抹平”分支痕迹；而 `--no-ff` 能让项目历史更清晰、可追溯。

------

## **🎯 使用场景**

- 团队协作开发，需要明确区分“功能开发”与“主干发布”
- 项目需要保留完整的合并历史（如审计、回溯、CI/CD 触发）
- 发布版本时，希望在 `main` 上看到清晰的“这是一个合并点”
- 遵循 Git Flow 或类似分支模型的项目

> ✅ 推荐：**所有从功能分支合并到主干的操作，都使用 `--no-ff`**

------

## **🛠️ 标准操作流程（**`--no-ff` **合并）**

```shell
# 1. 创建并切换到开发分支
git switch -c dev

# 2. 开发并提交
# （修改文件）
git add .
git commit -m "add merge"

# 3. 切回主干
git switch main

# 4. 使用 --no-ff 合并（强制生成合并提交）
git merge --no-ff -m "merge dev into main" dev

# 5. 查看带分支图的历史
git log --graph --pretty=oneline --abbrev-commit

# 6. 删除已合并的分支（可选）
git branch -d dev
```

> 🔍 合并后，`git log --graph` 会显示明显的分叉与合并结构，证明 `dev` 曾存在。

------

## **🚀 日常工作流 · 极简指令汇总**

```shell
# 功能开发
git switch -c feature/login
# ... coding ...
git add .
git commit -m "feat: login"

# 合并到 dev（团队共享开发分支）
git switch dev
git merge --no-ff -m "feat: login" feature/login
git branch -d feature/login

# 发布时合并 dev 到 main
git switch main
git merge --no-ff -m "release v1.0" dev
git push
```

> ✅ 命名建议：
>
> - 主干：`main`
> - 开发集成分支：`dev`
> - 功能分支：`feature/xxx`
> - 修复分支：`fix/xxx`

------

## **⚠️ 特殊情况处理**

```shell
# 如果忘记写 -m，Git 会自动打开编辑器让你输入合并提交信息
git merge --no-ff dev

# 若合并后想撤销（尚未推送）
git reset --hard HEAD~1

# 查看哪些提交来自哪个分支（配合 --graph 更清晰）
git log --oneline --graph --all

# 禁用 Fast-forward 作为默认行为（可选配置）
git config --global merge.ff false
# 之后 git merge 自动等价于 git merge --no-ff
```

> 💡 注意：
>
> - `--no-ff` 会多出一个“合并提交”，历史更真实，但提交数略多
> - Fast-forward 适合个人小项目；`--no-ff` 适合团队或需审计的项目
> - 合并提交的 `-m` 信息建议写明“合并了什么”，如 `"Merge feature/auth"`

------

> ✅ **记住口诀**：
> **“主干只发布，开发在 dev；合并用 --no-ff，历史看得清。”**