# **🏷️ Git 标签（Tag）：为重要版本打上“人话名字”**

## **📌 这篇文章是干什么的？**

本文介绍 **如何使用 Git 标签（tag）标记关键历史节点（如软件发布版本）**，用有意义的名称（如 `v1.0`）替代难记的 commit ID（如 `f52c633`）。

> 💡 标签 = 永久快照指针
>
> - 分支会移动，标签永不改变
> - 专用于标记**稳定、可发布的版本**

------

## **🎯 使用场景**

- 软件正式发布（v1.0、v2.3.1）
- 里程碑交付（alpha、beta、rc）
- 需要精确回滚到某个历史状态
- 自动化部署脚本指定版本源

> ✅ 推荐：所有对外发布的版本都应打 tag！

------

## **🛠️ 标准操作流程**

```shell
# 1. 切换到目标分支（通常是 main/master）
git switch main

# 2. 为最新提交打轻量标签（简单场景）
git tag v1.0

# 3. 为历史提交补打标签
git tag v0.9 f52c633   # f52c633 是 commit ID

# 4. 创建带说明的附注标签（推荐用于正式发布）
git tag -a v1.0 -m "Release version 1.0" 

# 5. 查看所有标签
git tag

# 6. 查看标签详情（含 commit 和说明）
git show v1.0
```

> 🔍 轻量标签 vs 附注标签：
>
> - `git tag v1.0` → 轻量标签（仅指针）
> - `git tag -a v1.0 -m "..."` → 附注标签（含作者、日期、说明，**推荐**）

------

## **🚀 日常工作流 · 极简指令汇总**

```shell
# 发布前：确保在正确分支
git switch main
git pull

# 打正式发布标签
git tag -a v1.2.0 -m "Stable release for production"

# 验证
git show v1.2.0

# （后续）推送到远程（需单独操作）
git push origin v1.2.0
# 或推送所有标签
git push origin --tags
```

> ✅ 命名建议：遵循 [语义化版本](https://semver.org/) 规范，如 `v1.0.0`、`v2.1.3-rc1`

------

## **⚠️ 特殊情况处理**

```shell
# 删除本地标签（谨慎！）
git tag -d v0.9

# 删除远程标签（先删本地，再推空引用）
git push origin :refs/tags/v0.9

# 列出标签并按时间排序（非默认字母序）
git tag --sort=-creatordate

# 检出某个标签的代码（只读，HEAD 处于 detached 状态）
git checkout v1.0
```

> 💡 注意：
>
> - 标签**不会随分支自动推送**，必须显式 `git push origin <tag>`
> - 同一个 commit 可打多个标签
> - 标签与分支无关——只要 commit 存在，标签就有效

------

> ✅ **记住口诀**：
> **“发布打标签，名字要规范；
> 轻量做草稿，附注上生产。”**