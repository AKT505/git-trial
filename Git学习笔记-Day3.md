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

## 常用命令速查

| 命令 | 用途 |
|------|------|
| `git cherry-pick <hash>` | 挑选特定提交到当前分支 |
| `git cherry-pick --abort` | 中止 cherry-pick |
| `git cherry-pick --continue` | 解决冲突后继续 cherry-pick |
| `git cherry-pick --skip` | 跳过当前提交 |
