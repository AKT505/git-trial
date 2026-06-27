# Git 学习笔记 — Day2（2026-06-27）

## 第七课：Clone & Pull

### `git clone` — 克隆远程仓库
```bash
git clone <仓库地址> [本地目录名]
```
- 把远程仓库完整复制到本地（包含全部提交历史）
- 自动配置 `origin` 远程地址，无需手动 `git remote add`
- 常见用途：新电脑拉项目、参与开源项目、团队成员入职

### `git pull` — 拉取远程更新
```bash
git pull origin <分支名>
```
- `git pull` = `git fetch` + `git merge`
- 下载远程更新并自动合并到当前分支

### `git fetch` — 仅下载不合并
```bash
git fetch origin
```
- 只下载远程更新，不修改工作区文件
- 更安全：先下载 → 检查变更 → 再决定是否合并
- **最佳实践**：先用 `git fetch` 看看有什么更新，再 `git merge` 或 `git pull`

| 命令 | 下载 | 合并 | 安全性 |
|------|:----:|:----:|:------:|
| `git fetch` | ✅ | ❌ | 安全 |
| `git pull` | ✅ | ✅ | 可能冲突 |

---

## 第八课：Advanced 命令

### `git stash` — 暂存未完成的工作
```bash
git stash              # 暂存所有未提交的修改
git stash list         # 查看暂存列表
git stash pop          # 恢复最近一次暂存，并删除记录
git stash apply        # 恢复最近一次暂存，保留记录
git stash drop         # 删除指定暂存
```
**场景**：代码改到一半，需要紧急切分支修 bug，但又不想提交半成品。

### `git rebase` — 变基
```bash
git rebase <目标分支>
```
- 把你的 commit 拆下来，重新"接"到目标分支顶端
- 结果：提交历史变成一条直线（无分叉）
- 对比：
  - `git merge`：产生 merge commit，保留分叉历史
  - `git rebase`：重写历史，变成线性

```
Before rebase:          After rebase:
* A (你的提交)          * A' (你的提交 rebase后)
| * B (main)            * B (main)
|/                      |
* C                     * C
```

⚠️ **注意**：不要 rebase 已经 push 到远程的公共分支，会搞乱别人的历史。

### `git cherry-pick` — 挑选提交（待学）

---

## GitHub 协作流程总结

```
1. git clone <url>           → 克隆仓库
2. git switch -c <分支名>    → 创建功能分支
3. 写代码，git add，git commit → 提交修改
4. git push -u origin <分支> → 推送分支到 GitHub
5. 在 GitHub 创建 Pull Request → 请求合并
6. Code Review               → 审查代码
7. Merge Pull Request        → 合并到 main
8. git switch main && git pull → 本地同步
9. git branch -d <分支名>    → 删除已合并的分支
```

---

## 常用命令速查

| 命令 | 用途 |
|------|------|
| `git clone <url>` | 克隆仓库 |
| `git pull origin main` | 拉取并合并远程更新 |
| `git fetch origin` | 仅下载远程更新 |
| `git stash` | 暂存修改 |
| `git stash pop` | 恢复暂存 |
| `git rebase main` | 变基到 main |
| `git branch -d <name>` | 删除分支 |
| `git push -u origin <name>` | 推送分支并设置上游 |
