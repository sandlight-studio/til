# 学习笔记（2025‑06‑18 ~ 2025‑06‑19 凌晨）
> **主题**：主题涵盖 Git Worktree、多账户 GitHub 配置、Python venv/pyenv 管理、WSL2 踩坑排查
## 1. Git & GitHub 进阶
### 1.1 `git worktree` 多分支并行
- **核心命令**  
  ```bash
  # 基于 master 创建并检出 hotfix 分支到 ./hotfix 目录
  git worktree add -b hotfix ./hotfix master
  
  # 查看所有工作树
  git worktree list
  
  # 删除工作树
  git worktree remove ./hotfix
  ```
- **适用场景**：主分支开发途中需紧急修复；减少 `stash`/切分支往返。

### 1.2 仓库转移后本地同步
```bash
# 更新 origin
git remote set-url origin git@github.com:sandlight-studio/til.git
# 若需重命名目录
mv ~/code/til ~/code/sandlight-til
```
> 🎯 提醒：确保 GitHub 组织已接受转移，否则 CLI 报 422。

### 1.3 “Choose a license” 速览

| 许可证 | 传播性 | 商用 | 专利许可 | 适用建议 |
|--------|--------|------|---------|---------|
| MIT | 宽松 | ✅ | ❌ | 库、脚手架 |
| Apache‑2.0 | 宽松 | ✅ | ✅ | 企业友好 |
| GPL‑3.0 | 传染 | ✅ | ✅ | 保证衍生品开源 |
| BSD‑3 | 宽松 | ✅ | ❌ | 学术项目 |
| MPL‑2.0 | 文件级 | ✅ | ✅ | 浏览器插件 |
| CC0 | 公共领域 | ✅ | ❌ | 数据集/示例 |

---

## 2. Python 开发环境（WSL）

### 2.1 venv 创建失败 (`ensurepip`)
- 解决：`sudo apt install python3.10-venv` ➜ 重新 `python3 -m venv .venv`.

### 2.2 VS Code 连接 WSL 提示 “Permission denied”
- 原因：`code` 可执行位于 NTFS；`/tmp` 权限冲突。  
  解决：  
  ```bash
  sudo chown $USER:$USER /tmp
  # 或将 VS Code 安装至 C:\Users\Jimmy\AppData\
  ```

### 2.3 升级项目 Python 版本
- 使用 **pyenv** 创建全局 3.12.3  
  ```bash
  pyenv install 3.12.3 && pyenv global 3.12.3
  ```
- 在 `pyproject.toml` 中调整  
  ```toml
  [project]
  requires-python = ">=3.12"
  ```
- 触发 CI 改用新解释器。

---

## 3. WSL 用户 & Node 环境

| 任务 | 命令 |
|------|------|
| 设置默认登录用户 | `ubuntu config --default-user jimmy` |
| 临时以 root 登陆 | `wsl -u root` |
| 安装 `nvm` | `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash` |
| 切换 Node 版本 | `nvm use 20` |

---

## 4. 即战行动计划（未来 7 天）

1. **公开项目** – 将 `ai-testcase-gen` 初始化并写首篇 README；
2. **LeetCode x Python** – 每日 2 题并汇总至 `til/leetcode`.
3. **反馈循环** – 每晚 22:30 回顾当天 commit log，写 3 行 retro。

---