# 夸克网盘技能

纯 Python 实现的夸克网盘操作技能，支持扫码登录、文件管理等功能。

## 功能

- ✅ 扫码登录（获取 Cookie）
- ✅ 查看用户信息和容量
- ✅ 列出文件/目录
- ✅ 搜索文件
- ✅ 上传文件
- ✅ 下载文件
- ✅ 删除文件/文件夹
- ✅ 清空目录
- ✅ 创建文件夹

## 快速开始

### 1. 安装依赖

```bash
pip install httpx
```

### 2. 登录

```bash
python3 scripts/quark_cli.py login
```

### 3. 使用

```bash
# 查看用户信息
python3 scripts/quark_cli.py user

# 列出根目录文件
python3 scripts/quark_cli.py list /

# 搜索文件
python3 scripts/quark_cli.py search "关键词"

# 上传文件
python3 scripts/quark_cli.py upload /local/file.txt /remote/file.txt

# 下载文件
python3 scripts/quark_cli.py download /remote/file.txt /local/file.txt

# 删除文件
python3 scripts/quark_cli.py delete /remote/file.txt

# 清空目录
python3 scripts/quark_cli.py clear /remote/folder

# 创建文件夹
python3 scripts/quark_cli.py mkdir "新文件夹"
```

## Cookie 配置

### 方式1：环境变量

```bash
export QUARK_COOKIE="your_cookie_string"
```

### 方式2：配置文件

```bash
python3 scripts/quark_cli.py config --cookie "your_cookie_string"
```

## API 参考

- [扫码登录流程](references/auth.md)
- [文件操作 API](references/file_ops.md)

## 技术实现

- **语言**: Python 3.7+
- **HTTP 客户端**: httpx
- **认证方式**: Cookie（`__pus`）
- **API 基础 URL**: `https://drive-pc.quark.cn`

## 已知限制

1. Cookie 有效期约 7 天，过期后需重新登录
2. 单文件上传大小限制：40GB（超级会员）
3. 上传功能需要完善 COS 上传逻辑

## 参考项目

- [kuake_cli](https://github.com/zhangjingwei/kuake_cli) - Go 语言实现
- [QuarkPan](https://github.com/lich0821/QuarkPan) - Python 实现

## License

MIT
