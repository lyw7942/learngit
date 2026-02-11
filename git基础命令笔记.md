# 设置全局用户名和邮箱

```shell
# 设置全局用户名和邮箱
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# 查看配置
git config --list
git config user.name          # 查看用户名
git config user.email         # 查看邮箱
```



# 新建一个git仓库，并上传文及修改后再上传

```shell
#先在指定位置创建一个文件夹
cd /f
mkdir  my-git-project

#把当前git命令中的路径设置为需要变为仓库的路径
cd my-git-project

#将当前文件位置初始化为git仓库（重要）
git init

#验证是否初始化成功
ls -la
------------------------------初始化完毕，开始上传文件---------------------------------------
#创建一个示例文件
echo "# My First Git Project" > README.md
echo "This is a project to learn Git." >> README.md

(可选：#查看工作区文件的状态，是否被git跟踪到了）
git status

#将文件从工作区转移到暂存区
git add README.md

(可选：#再次查看状态，是否添加在暂存区了，等待被commit）
git status

#将文件从暂存区移动到git仓库
git commit -m"首次提交"

(可选：#查看提交记录）
git log --oneline
------------------------------成功上传了文件后，希望改动该文件再次上传新文件----------------------
#修改现有文件
echo "" >> README.md
echo "## Project Features" >> README.md
echo "- Learn Git basics" >> README.md
echo "- Practice version control" >> README.md

#查看工作区和暂存区的差别
git diff

#再次添加到仓库
git add README.md
git commit -m"第二次提交"
```



# 撤销暂存区，工作区

```shell
# === 场景1：单文件被修改，未add和commit===
# 目标：丢弃工作区修改，恢复为 HEAD 中的版本
git restore readme.txt           


# === 场景2：单文件被修改，已add，但未commit===
# 目标：取消暂存 + 丢弃工作区内容
git restore --staged readme.txt  # 取消暂存（unstage）
git restore readme.txt           # 丢弃工作区修改

#或者（一步到位）
git restore --staged --worktree readme.txt
```



# 整个项目回退版本（可选择性保留当前工作区和暂留区）

```shell
# === 场景1：整个项目回退到上一版本（已 commit）===
git log --oneline                # 查看提交历史
git reset --hard HEAD~1          # 回退到上一个提交（丢弃最近一次 commit）

git reflog                       # 若后悔，查看 HEAD 操作记录
git reset --hard <commit-id>     # 恢复到指定版本（如 1094adb）
```



# 删除某个已经跟踪过的文件

```shell
# === 场景1：确实要从版本库中删除已跟踪的文件 ===
rm readme.txt					# 从操作系统中删除readme.txt文件
git status						# (可选)
git rm readme.txt				# 将“删除”操作添加到暂存区（相当于git add readme.txt）
git commit -m"成功删除readme.txt"# 提交删除

```

