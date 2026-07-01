# Git 学习笔记 — Day3（2026-07-01）

## 第十三课：`git cherry-pick` — 挑选特定提交

### 基本用法
```bash
git cherry-pick <commit-hash>
```
- 把某个指定的提交"复制"到当前分支
- 不会把其他无关的提交带过来
- 是**复制**不是移动，新提交会有不同的 hash

### 场景
```
main:     A---B---C
                   \
feature-a:          D---E---F

想把 E 单独拿到 main →
git switch main
git cherry-pick E的hash

结果：main: A---B---C---E'
```

### 常用参数
```bash
git cherry-pick <hash1> <hash2>       # 挑选多个提交
git cherry-pick <start>..<end>        # 挑选一个范围（不含 start）
```

### 冲突处理
```bash
git cherry-pick --abort               # 中止，回到 cherry-pick 之前
git cherry-pick --continue            # 解决冲突后继续
git cherry-pick --skip                # 跳过这个提交，继续下一个
```

### ⚠️ 实际遇到的情况
- 如果忘了切回目标分支，在源分支上 cherry-pick 自己的提交 → 会冲突
- 先 `git cherry-pick --abort` 中止，切到正确分支再操作

---

## 第十四课：解决合并冲突（实战）

### 冲突标记的阅读方法
```
<<<<<<< HEAD          ← 当前分支的内容
  (你自己的代码)
=======               ← 分隔线
  (cherry-pick/merge 想加进来的代码)
>>>>>>> 226a4be       ← 来源提交的 hash
```

### 解决步骤
1. 编辑冲突文件，删掉 `<<<<<<<`、`=======`、`>>>>>>>` 三行标记
2. 保留你想要的内容（通常是两边都保留）
3. `git add <文件名>` — 告诉 Git 冲突已解决
4. `git cherry-pick --continue`（或 `git merge --continue`）— 继续操作

### 中止操作
| 命令 | 用途 |
|------|------|
| `git cherry-pick --abort` | 中止 cherry-pick |
| `git merge --abort` | 中止 merge |
| `git rebase --abort` | 中止 rebase |

---

## 第十五课：`git log` 高级筛选

### 常用搜索命令
```bash
git log --oneline --author="名字"     # 按作者筛选提交
git log --oneline --grep="关键词"     # 搜索提交信息中的关键词
git log --oneline --since="日期"      # 从某日期开始
git log --oneline --until="日期"      # 到某日期为止
git log --oneline -3                  # 只看最近 3 条
git log --oneline -- <文件名>         # 只看某个文件的提交历史
git log --stat -3                     # 最近 3 条 + 增删统计
git log --oneline --graph --all       # 图形化显示所有分支
```

### 组合使用
多个参数可以同时使用：
```bash
git log --oneline --author="AKT" --since="2026-06-01" -- feature.txt
```
查找某个作者在某日期之后对某文件的所有提交。

---

## 第十六课：`.gitignore` 文件

### 作用
忽略不需要 Git 跟踪的文件：
- 编译产物：`*.exe`、`*.class`、`node_modules/`
- 密钥文件：`*.env`、`*.key`
- 系统垃圾：`Thumbs.db`、`.DS_Store`
- 日志/临时文件：`*.log`、`*.tmp`

### 规则语法
| 规则 | 含义 | 示例匹配 |
|------|------|----------|
| `*.log` | 所有 `.log` 结尾的文件 | `debug.log`、`error.log` |
| `*.env` | 所有 `.env` 结尾的文件 | `secret.env`、`.env` |
| `temp/` | 整个目录 | `temp/` 下的所有文件 |
| `.env` | 仅名为 `.env` 的文件 | `.env`（不匹配 `app.env`） |
| `# 注释` | 注释行，被忽略 | — |

### ⚠️ 注意事项
- 规则每行前面**不能有空格**
- `.gitignore` 本身需要提交到仓库
- 已经跟踪过的文件不会被 `.gitignore` 忽略（需要先 `rm --cached`）

---

## 编辑器配置

```bash
git config --global core.editor "code --wait"
```
- 以后 `git commit`（不带 `-m`）、`git rebase -i` 等操作会在 VS Code 中打开
- 保存并关闭文件 = 确认，无需面对 Vim

---

## 常用命令速查

| 命令 | 用途 |
|------|------|
| `git cherry-pick <hash>` | 挑选特定提交到当前分支 |
| `git cherry-pick --abort` | 中止 cherry-pick |
| `git cherry-pick --continue` | 解决冲突后继续 cherry-pick |
| `git cherry-pick --skip` | 跳过当前提交 |
| `git log --author="名字"` | 按作者筛选提交 |
| `git log --grep="关键词"` | 按提交信息搜索 |
| `git log --since="日期"` | 从某日期开始的提交 |
| `git log --stat` | 显示每个提交的增删统计 |
| `git log -- <文件>` | 查看某文件的提交历史 |
| `git config --global core.editor "code --wait"` | 设置 VS Code 为默认编辑器 |
