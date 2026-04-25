# V1-fix Node.js 版本部署指南

## 概述

V1-fix 已成功转换为 Node.js 项目，并已配置支持在 Vercel 上部署。此版本保留了原有的所有功能特性。

## 📋 环境要求

- Node.js >= 18.0.0
- npm 或 yarn

## 🚀 快速开始

### 1. 本地开发

```bash
# 安装依赖
cd /path/to/meting/V1-fix
npm install

# 运行开发服务器
npm run start:node

# 服务将启动在 http://localhost:3000
```

### 2. Vercel 部署

#### 方式 A：自动部署（推荐）

1. 将项目上传到 GitHub
2. 访问 [Vercel Dashboard](https://vercel.com/dashboard)
3. 点击 "Add New" → "Project"
4. 导入你的 GitHub 仓库
5. 确保选择正确的项目根目录（如果在子文件夹中）
6. 点击 "Deploy"

#### 方式 B：Vercel CLI 部署

```bash
# 安装 Vercel CLI
npm install -g vercel

# 部署到 Vercel
vercel

# 用于生产环境
vercel --prod
```

### 3. Docker 部署

```bash
# 构建镜像
docker build -t meting-v1-fix .

# 运行容器
docker run -d -p 3000:3000 --name meting meting-v1-fix

# 查看日志
docker logs -f meting
```

## 📁 项目结构

```
V1-fix/
├── api/                      # Vercel 函数入口点
│   └── index.js             # 处理所有请求的入口
├── src/
│   ├── config.js            # 应用配置
│   ├── providers/           # 音乐源提供者
│   │   ├── netease/        # 网易云音乐
│   │   ├── tencent/        # QQ 音乐
│   │   └── index.js        # 提供者管理
│   ├── service/
│   │   └── api.js          # API 业务逻辑
│   ├── util.js             # 工具函数
│   └── template.js         # 测试页面模板
├── app.js                   # Hono Web 应用
├── node.js                  # Node.js 启动脚本
├── vercel.json              # Vercel 配置
├── package.json             # 项目配置
├── Dockerfile               # Docker 配置
├── cache/                   # 缓存目录（可选）
└── README.md                # 项目文档
```

## ⚙️ 环境变量配置

在 Vercel 部署时，可以设置以下环境变量：

| 变量名 | 说明 | 默认值 | 可选值 |
|--------|------|--------|--------|
| `OVERSEAS` | 是否部署在国外 | auto | 0, 1 |
| `PORT` | 监听端口 | 3000 | 任意端口号 |

### 在 Vercel 中设置环境变量

1. 进入项目设置 → Environment Variables
2. 添加需要的环境变量
3. 重新部署项目

## 📊 API 接口

### 基础 URL

```
/api?server=:server&type=:type&id=:id
```

### 支持的 server 参数

- `netease` - 网易云音乐
- `tencent` - QQ 音乐

### 支持的 type 参数

| type | 说明 | 返回格式 |
|------|------|---------|
| `song` | 获取单曲信息 | JSON 数组 |
| `playlist` | 获取歌单信息 | JSON 数组 |
| `search` | 搜索音乐 | JSON 数组 |
| `url` | 获取播放链接 | 重定向或文本 |
| `pic` | 获取专辑图片 | 重定向 |
| `lrc` | 获取歌词 | 纯文本 |

### 使用示例

```bash
# 获取网易云音乐单曲
curl "http://localhost:3000/api?server=netease&type=song&id=1396947846"

# 获取 QQ 音乐歌单
curl "http://localhost:3000/api?server=tencent&type=playlist&id=5165145936"

# 搜索音乐
curl "http://localhost:3000/api?server=netease&type=search&keyword=周杰伦"

# 获取音乐播放链接（返回 302 重定向）
curl -L "http://localhost:3000/api?server=netease&type=url&id=1396947846"
```

## 🔍 测试

部署成功后，访问以下链接验证：

- 主页：`https://your-domain.com/`
- 测试页面：`https://your-domain.com/test`
- API 接口：`https://your-domain.com/api?server=netease&type=song&id=1396947846`

## 🔧 故障排除

### 问题 1：Vercel 部署失败

**原因**：依赖安装失败或配置错误

**解决方案**：
1. 检查 `package.json` 是否在根目录
2. 检查 `vercel.json` 配置是否正确
3. 查看 Vercel 部署日志获取详细错误

### 问题 2：QQ 音乐无法获取歌曲信息

**原因**：可能涉及地区限制或 API 变更

**解决方案**：
1. 如果部署在海外，设置 `OVERSEAS=1`
2. 检查 QQ 音乐是否有更新的 API 限制
3. 查看项目 Issues 获取最新信息

### 问题 3：速度慢

**原因**：
- Vercel 冷启动时间较长
- 国内访问国外服务器网络延迟
- DNS 解析慢

**解决方案**：
1. 使用自定义域名代替 `*.vercel.app` 域名
2. 配置 CDN 加速
3. 定期访问保持实例温暖

### 问题 4：歌词返回为空

**原因**：某些歌曲可能没有歌词数据

**解决方案**：
1. 这是正常现象，某些歌曲确实没有歌词
2. 尝试其他歌曲
3. 某些歌词可能需要特殊权限

## 📦 相关文件说明

### vercel.json

```json
{
    "env": {
        "RUNTIME": "vercel"
    },
    "rewrites": [
        {
            "source": "/(.*)",
            "destination": "/api"
        }
    ]
}
```

- `env.RUNTIME`: 标识运行时环境为 Vercel
- `rewrites`: 将所有请求转发到 `/api` 处理

### app.js

应用主文件，使用 Hono Web 框架处理请求。

### api/index.js

Vercel 函数入口，处理 HTTP 请求并转发给 Hono 应用。

## 🔐 安全考虑

1. **CORS**: 默认允许所有源的 CORS 请求
2. **速率限制**: 建议在生产环境配置速率限制
3. **日志**: 所有请求都被记录，可在 Vercel 控制面板查看

## 📝 版本信息

- **项目名**: meting-v1-fix-nodejs
- **基础版本**: V1 (Node.js Meting API)
- **当前版本**: 1.0.0
- **Node.js**: >= 18.0.0

## 🔗 相关资源

- [Hono 文档](https://hono.dev/)
- [Vercel 文档](https://vercel.com/docs)
- [原始 Meting 项目](https://github.com/metowolf/Meting-API)

## ✨ 特性

- ✅ 支持网易云音乐和 QQ 音乐
- ✅ 完整的 API 功能（单曲、歌单、搜索、歌词等）
- ✅ 支持多种部署方式（本地、Docker、Vercel）
- ✅ 生产级别的性能和稳定性
- ✅ 简洁的代码结构，易于维护和扩展

## 📞 获取帮助

如遇问题，请：
1. 查阅本文档中的故障排除章节
2. 查看项目的 Issues
3. 在 Vercel 社区寻求帮助

---

**祝部署顺利！** 🎉
