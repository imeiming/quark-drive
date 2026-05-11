---
name: quark-drive
description: |
  夸克网盘操作技能 - 支持扫码登录、文件列表、上传、下载、分享（可设密码/有效期）、转存、删除等操作。
  当用户提到夸克网盘、quark、网盘文件管理、上传下载、分享链接、转存资源时使用此技能。
metadata:
  openclaw:
    emoji: ☁️
    requires:
      bins: ['python3']
      env:
        - QUARK_COOKIE
---

# 夸克网盘操作技能

通过 Python 脚本操作夸克网盘，支持扫码登录、文件管理、分享、转存等功能。

## ⚠️ 使用前必读

1. **首次使用必须先登录**：运行 `python3 scripts/quark_cli.py login` 获取 Cookie
2. **Cookie 有效期**：约 7 天，过期后需重新登录
3. **文件大小限制**：单文件最大 40GB（超级会员）
4. **并发限制**：避免同时进行多个上传/下载任务

## 功能路由表

| 用户需求 | 命令 | 说明 |
|---------|------|------|
| "登录夸克网盘" | `python3 scripts/quark_cli.py login` | 扫码登录获取 Cookie |
| "列出文件" | `python3 scripts/quark_cli.py list [路径]` | 列出目录内容 |
| "搜索文件" | `python3 scripts/quark_cli.py search <关键词>` | 搜索文件 |
| "上传文件" | `python3 scripts/quark_cli.py upload <本地路径> <网盘路径>` | 上传文件 |
| "下载文件" | `python3 scripts/quark_cli.py download <网盘路径> <本地路径>` | 下载文件 |
| "删除文件" | `python3 scripts/quark_cli.py delete <路径>` | 删除文件/文件夹 |
| "清空文件" | `python3 scripts/quark_cli.py clear [路径]` | 清空指定目录 |
| "创建文件夹" | `python3 scripts/quark_cli.py mkdir <名称> [父目录]` | 创建文件夹 |
| "查看用户信息" | `python3 scripts/quark_cli.py user` | 显示用户信息和容量 |
| "分享文件" | `python3 scripts/quark_cli.py share <路径> --expire 7d --passcode 1234` | 创建分享链接（可设密码） |
| "查看我的分享" | `python3 scripts/quark_cli.py share-list` | 列出所有分享 |
| "取消分享" | `python3 scripts/quark_cli.py share-cancel <分享ID或链接>` | 取消分享 |
| "转存资源" | `python3 scripts/quark_cli.py share-save <分享链接> --to /目标目录` | 转存他人分享到我的网盘 |

## 分享功能详解

### 创建分享（带密码）
```bash
# 无密码分享，7天有效
python3 scripts/quark_cli.py share /文档/report.pdf --expire 7d

# 带密码分享，30天有效
python3 scripts/quark_cli.py share /文档/report.pdf --expire 30d --passcode 1234

# 永久分享
python3 scripts/quark_cli.py share /文档/report.pdf --expire permanent
```

### 转存他人分享
```bash
# 基本转存
python3 scripts/quark_cli.py share-save "https://pan.quark.cn/s/xxxxx"

# 带密码转存
python3 scripts/quark_cli.py share-save "https://pan.quark.cn/s/xxxxx 提取码: 1234"

# 指定保存目录
python3 scripts/quark_cli.py share-save "https://pan.quark.cn/s/xxxxx" --to /我的资源
```

## Cookie 配置

### 方式1：环境变量（推荐）
```bash
export QUARK_COOKIE="your_cookie_string_here"
```

### 方式2：配置文件
```bash
python3 scripts/quark_cli.py config --cookie "your_cookie_string_here"
```

### 方式3：扫码登录（推荐）
```bash
python3 scripts/quark_cli.py login
```

## 错误处理

| 错误码 | 说明 | 解决方案 |
|--------|------|----------|
| 401 | 未授权 | 重新登录 |
| 403 | 禁止访问 | 检查文件权限 |
| 404 | 文件不存在 | 检查路径 |
| 413 | 文件过大 | 分片上传或压缩 |
| 500 | 服务器错误 | 稍后重试 |

## 技术实现

- **API 基础 URL**: `https://drive-pc.quark.cn`
- **认证方式**: Cookie（`__pus`）
- **上传方式**: 分片上传（支持断点续传）
- **下载方式**: 直链下载
- **分享协议**: 支持密码/有效期设置
