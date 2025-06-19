
---
title: 📝 学习笔记（2025‑06‑19 ~ 2025‑06‑20）
date: 2025‑06‑20
description: Git 凭据与 SSH、API 密钥分类、Spring Retry + 分布式锁、RabbitMQ 重试、Gradle 镜像及云服务对比
---

> **时间窗口**：2025‑06‑19 00:00 — 2025‑06‑20 01:00  
> **与前日区别**：本笔记聚焦新的疑难与配置

## 1. Git 凭据与密钥管理（WSL）

### 1.1 解决 GDBus/Secrets 报错
```bash
sudo apt-get install -y dbus-x11   libsecret-1-0 libsecret-1-dev

# 重新编译 Git libsecret helper（如需）
cd /usr/share/doc/git/contrib/credential/libsecret
sudo make
git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret
```
> **效果**：没生效，最后是删除了，使用SSH Key

### 1.2 SSH Key 流程（ed25519）
```bash
ssh-keygen -t ed25519 -C "jimmy@example.com"
eval "$(ssh-agent -s)" && ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub | clip.exe   # WSL
ssh -T git@github.com
```
- 建议同时设置 **签名密钥** 与 **登录密钥**，保持提交可信。

### 1.3 常用 Git Alias
| Alias | 命令 | 用途 |
|-------|------|------|
| `gf`  | `git fetch` | 快速拉取 |
| `gs`  | `git status -sb` | 精简状态 |
| `gco` | `git checkout` | 切分支 |

## 2. .gitignore 与目录迁移

- 调整忽略规则后执行  
  ```bash
  git rm -r --cached .
  git add . && git commit -m "fix: refresh .gitignore"
  ```
- 本地项目移动至 `/sandlight-studio`。

---

## 3. API 密钥类型速览

| 类型 | 典型载体 | 特征 | 适用场景 |
|------|----------|------|---------|
| **Basic** | `Authorization: Basic ...` | 明文编码 | 内部系统 |
| **Bearer** | `Authorization: Bearer ...` | Opaque | OAuth2 |
| **JWT** | 三段式 Token | 自含声明 | 微服务鉴权 |
| **HMAC** | Header/Query + HMAC | 单次签名 | 金融 |
| **Signed URL** | URL 签名+过期 | 无 Header | CDN |

---

## 4. Spring Retry × Redis 锁

- 私有方法事务失效 → 需通过接口层调用或拆分成新 Bean。
- `RetryTemplate` + `LockComponent.doInLockBy` 组合：在重试前拿业务锁，保证幂等。

---

## 5. RabbitMQ Listener 重试

- 手动 ack / nack，结合 Redis 锁做 idempotent。
- 考虑延迟插件或死信队列管理重试次数。

---

## 6. Gradle 镜像与 Nexus

- 在 `settings.gradle.kts` 配置公司 Nexus 源及凭据。
- 对插件仓库使用代理，减少外网依赖。

---

## 7. 海外云服务对比要点

| 云厂商 | 免费额度 | 网络延迟 (🇨🇳→SG) | AI 服务 | 个人注册难度 |
|--------|----------|------------------|---------|--------------|
| Google Cloud | \$300/90d | 60‑80 ms | Vertex AI / Gemini | 严 |
| AWS | 12 mo 免费层 | 50‑70 ms | Bedrock/SageMaker | 中 |
| Azure | \$200/30d | 60‑80 ms | OpenAI | 中 |
| 阿里云 | 试用券 | 30‑40 ms | 百炼/PAI | 易 |

---

## 8. 下一步待办

- Python学习
- 熟练 Cursor + Claude Code 基本流
- Google Cloud 继续调研

---

**Break assumptions. Build reliably.** 🚀
