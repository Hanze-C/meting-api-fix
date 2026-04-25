# 🎵 V1-fix Node.js 版本 - 快速参考

## ✨ 转换完成

V1-fix 已从 PHP 转换为 Node.js + Hono 框架，完全支持 Vercel 部署。

## 🚀 3 分钟快速开始

### 1️⃣ 本地运行
```bash
cd V1-fix
npm install
npm run start:node
# 打开 http://localhost:3000
```

### 2️⃣ Vercel 部署
```bash
# 推送到 GitHub 后
npm install -g vercel
vercel --prod
```

### 3️⃣ Docker 运行
```bash
docker build -t meting-v1-fix .
docker run -d -p 3000:3000 meting-v1-fix
```

## 📡 API 使用

```bash
# 网易云单曲
/api?server=netease&type=song&id=1396947846

# QQ 音乐搜索
/api?server=tencent&type=search&keyword=周杰伦

# 获取歌词
/api?server=netease&type=lrc&id=1396947846

# 获取播放链接
/api?server=netease&type=url&id=1396947846
```

## 📁 关键文件

| 文件 | 作用 |
|------|------|
| `vercel.json` | Vercel 配置 |
| `api/index.js` | Vercel 函数入口 |
| `app.js` | Web 应用主文件 |
| `src/providers/` | 音乐源实现 |

## 📖 文档

- 📘 `DEPLOYMENT.md` - 完整部署指南
- 📗 `README-v1-fix-nodejs.md` - 项目说明
- 📙 `MIGRATION_SUMMARY.md` - 转换详情

## ⚙️ 环境变量（可选）

```bash
OVERSEAS=1      # 国外部署
PORT=3000       # 监听端口
```

## ✅ 功能清单

- ✅ 网易云音乐（单曲、歌单、搜索、歌词）
- ✅ QQ 音乐（单曲、歌单、歌词）
- ✅ 播放链接获取
- ✅ 专辑图片
- ✅ 跨域请求支持
- ✅ 错误处理

## 🔍 测试端点

```
主页：          http://localhost:3000/
测试页面：      http://localhost:3000/test
API 接口：      http://localhost:3000/api
```

## 💡 常用命令

```bash
# 开发
npm run start:node

# 构建
npm run build:all

# 测试
npm test

# 版本发布
npm run patch    # 补丁版本
npm run minor    # 小版本
npm run major    # 大版本
```

## 🆘 故障排除

| 问题 | 解决方案 |
|------|----------|
| `npm install` 失败 | 清除 `node_modules` 和 `package-lock.json` 后重新安装 |
| 部署到 Vercel 失败 | 检查 `vercel.json` 和 `package.json` 是否在根目录 |
| QQ 音乐无法获取 | 确保网络连接，考虑设置 `OVERSEAS=1` |
| 服务速度慢 | 在 Vercel 中使用自定义域名，避免 `*.vercel.app` |

## 🎯 下一步

1. ✅ 本地测试运行
2. ✅ 推送到 GitHub
3. ✅ 连接 Vercel 部署
4. ✅ 在前端使用 API

## 📞 获取帮助

- 📘 查看 `DEPLOYMENT.md` 的故障排除部分
- 🔍 检查 Vercel 控制面板日志
- 📧 查看项目 GitHub Issues

---

**状态**: ✅ 已转换  
**位置**: `/Downloads/meting/V1-fix/`  
**类型**: Node.js + Vercel  
**基础**: Hono Web Framework
