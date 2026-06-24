# Git 学习笔记 (Day 1)

> 学习日期：2026-06-24
> 仓库地址：https://github.com/AKT505/git-trial

---

## 一、Git 安装 & 右键菜单

通过 **Scoop** 安装的 Git 是便携版，不会自动注册 Windows 右键菜单。需要通过注册表手动添加：

- **Git Bash Here** — 在当前目录打开 Git Bash 终端
- **Git GUI Here** — 在当前目录打开 Git GUI 图形界面

推荐使用命令行（Git Bash）学习 Git。

---

## 二、基础配置

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

- `--global`：全局生效，所有仓库通用
- 不加 `--global`：仅当前仓库生效

---

## 三、核心工作流

```
工作区 (修改文件)  →  暂存区 (git add)  →  仓库 (git commit)
```

| 命令 | 作用 |
|---|---|
| `git init` | 初始化 Git 仓库 |
| `git status` | 查看当前状态（改了哪些文件） |
| `git diff` | 查看具体改了什么内容 |
| `git add <file>` | 将文件加入暂存区 |
| `git commit -m "消息"` | 将暂存区内容提交到仓库 |
| `git log` | 查看提交历史 |

### 常用 `git log` 变体

```bash
git log --oneline              # 简洁单行模式
git log --stat                 # 带文件变动统计
git log --oneline --graph --all # 图形化分支历史
```

---

## 四、分支 (Branch)

分支是 Git 最重要的概念之一——在不同分支上开发互不影响。

| 命令 | 作用 |
|---|---|
| `git branch` | 查看所有分支 |
| `git branch <name>` | 创建新分支 |
| `git switch <name>` | 切换到指定分支 |
| `git branch -d <name>` | 删除分支 |

---

## 五、合并 (Merge)

将分支上的工作合并回主线。

| 场景 | 合并方式 |
|---|---|
| 主线无新提交 | **Fast-forward**（快进），不产生新提交 |
| 两边都有新提交 | **Merge commit**，产生一个合并提交 |

```bash
git switch master       # 先切到目标分支
git merge <branch>      # 合并指定分支到当前分支
```

---

## 六、撤销 & 回退

Git 的三层数据模型：

```
工作区 ←→ 暂存区 ←→ 仓库
```

### 三种 reset 模式

| 模式 | 效果 | 安全性 |
|---|---|---|
| `git reset --soft HEAD~1` | 撤销 commit，改动留在暂存区 | ⭐⭐⭐ 最安全 |
| `git reset --mixed HEAD~1` | 撤销 commit，改动退到工作区 | ⭐⭐ 默认模式 |
| `git reset --hard HEAD~1` | 撤销 commit，改动直接丢弃 | ⚠️ 危险，不可恢复 |

### 其他撤销命令

| 命令 | 作用 |
|---|---|
| `git restore <file>` | 丢弃工作区改动（从暂存区恢复） |
| `git restore --staged <file>` | 取消暂存（从仓库恢复暂存区） |

---

## 七、远程仓库 (GitHub)

### SSH 配置

```bash
ssh-keygen -t ed25519 -C "你的邮箱"   # 生成密钥
cat ~/.ssh/id_ed25519.pub            # 查看公钥
ssh -T git@github.com                # 测试连接
```

公钥需添加到 GitHub → Settings → SSH and GPG keys。

### 推送代码

```bash
git remote add origin <url>   # 关联远程仓库
git branch -M main            # 重命名分支为 main
git push -u origin main       # 推送，-u 记住上游
```

之后日常使用只需要：

```bash
git push    # 推送到已关联的远程仓库
```

---

## 八、实用提示

- 在 VS Code 里写代码，在终端里执行 Git 命令——各司其职
- `code .` 可以在终端中直接打开当前目录到 VS Code
- 遇到分页器卡住按 `q` 退出
- 遇到编辑器卡住 `Esc` → `:q!` 回车强制退出

---

## 待学习

- `git pull` — 拉取远程更新
- `git clone` — 克隆远程仓库
- `git stash` — 暂存未完成的工作
- `git rebase` — 变基
- `git cherry-pick` — 挑选提交
