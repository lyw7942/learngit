# **Git 本地 ↔ GitHub 同步速查（HTTPS 方式）**

## **✅ 前提**

- 本地已 `git init` 并至少有一次 `commit`
- GitHub 已创建**空仓库**（不勾选 README）

------

## **🔧 核心操作**

```shell
# 1. 关联远程仓库（替换为你的用户名/仓库名）
git remote add origin https://github.com/lyw7942/仓库名.git

# 2. 验证远程地址（注意：-v 无空格！）
git remote -v

# 3. 首次推送（建立跟踪）
git push -u origin master
```

> 💡 首次推送会弹窗：
>
> - **Username**: GitHub 用户名
>   **用户名** ：GitHub 用户名
> - **Password**: [Personal Access Token](https://github.com/settings/tokens)（需勾选 `repo`）

------

## **⚙️ 常见问题解决**

### **网络屏蔽 SSH（Connection refused）？**

→ **直接用 HTTPS**（如上），无需 SSH。

### **SSL 证书错误？**

```
git config --global http.sslBackend schannel
```

### **重命名 GitHub 仓库后？**

```
git remote set-url origin https://github.com/用户名/新仓库名.git
```

------

## **🔄 日常使用**

```
git add .
git commit -m "msg"
git push          # 推送
git pull          # 拉取更新
```