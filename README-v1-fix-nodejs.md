# Meting-API (V1-fix Node.js 版本)

这是 V1-fix 的 Node.js 改造版本，保留了原有 V1-fix 项目的特点，现已支持在 Vercel 上部署。

## 版本说明

- **原 V1-fix**: 基于 PHP 的 Meting API 实现，功能完整
- **当前版本**: 基于 Node.js + Hono 框架的重构，支持 Vercel 部署

## 主要特性

- ✅ 基于 JavaScript 实现
- ✅ 插件系统，支持多个音乐源
- ✅ 支持网易云音乐和 QQ 音乐
- ✅ 可在 Vercel 上直接部署
- ✅ 缓存机制支持
- ✅ 腾讯音乐 Cookie 管理支持

## 音乐源支持

| 音乐源 | 参数名 | 图片 | 歌词 | 播放链接 | 单曲 | 歌单 | 歌手 | 搜索 |
|--------|--------|------|------|----------|------|------|------|------|
| 网易云 | netease | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |
| QQ音乐 | tencent | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ |

## 部署方式

### Vercel 部署（推荐）

最简单的方式是使用 Vercel 的一键部署：

<a href="https://vercel.com/import/project?template=https://github.com/YOUR_USERNAME/YOUR_REPO"><img src="https://vercel.com/button" height="36"></a>

或者手动部署：

1. Fork 或 clone 本项目
2. 连接到 Vercel
3. 点击 Deploy
4. 等待部署完成

### 本地运行

需要 Node.js >= 18.0.0

```bash
# 安装依赖
npm install

# 运行
npm run start:node

# 或使用 node 直接运行
node node.js
```

### Docker 部署

```bash
# 构建镜像
docker build -t meting-api .

# 运行容器
docker run -d -p 3000:3000 meting-api
```

## 配置

所有配置通过环境变量设置：

| 环境变量 | 说明 | 默认值 |
|---------|------|--------|
| `OVERSEAS` | 是否部署在国外（1/0） | 自动检测 |
| `PORT` | 监听端口 | 3000 |

### 国外部署

如果部署在国外服务器，需要设置 `OVERSEAS=1` 以启用 QQ 音乐的 JSONP 返回。

## 使用方式

### API 接口

```
GET /api?server=:server&type=:type&id=:id
```

**参数说明：**

- `server`: 音乐源，可选值 `netease`、`tencent`
- `type`: 类型，可选值 `song`、`playlist`、`url`、`pic`、`lrc`、`search`
- `id`: 资源 ID

**示例：**

```
# 获取网易云单曲信息
GET /api?server=netease&type=song&id=1396947846

# 获取网易云歌单
GET /api?server=netease&type=playlist&id=919246173

# 搜索网易云音乐
GET /api?server=netease&type=search&id=&keyword=刘德华

# 获取播放链接
GET /api?server=netease&type=url&id=1396947846

# 获取专辑图片
GET /api?server=netease&type=pic&id=71192683

# 获取歌词
GET /api?server=netease&type=lrc&id=1396947846
```

## 前端集成

在引入 Meting 前端插件前，设置 API 地址：

```html
<script>
  var meting_api = 'https://your-domain.com/api?server=:server&type=:type&id=:id';
</script>
<script src="https://cdn.jsdelivr.net/npm/meting@2.0.1/dist/Meting.min.js"></script>
```

## 相关项目

- [MetingJS](https://github.com/metowolf/MetingJS) - 前端插件
- [Meting-API](https://github.com/metowolf/Meting-API) - 原始项目
- [Hono](https://github.com/honojs/hono) - Web 框架
- [NeteaseCloudMusicApi](https://github.com/Binaryify/NeteaseCloudMusicApi) - 网易云 API

## License

MIT

## 文件结构

```
├── api/                    # Vercel 函数入口
├── src/
│   ├── config.js          # 配置文件
│   ├── providers/         # 音乐源提供者
│   │   ├── netease/      # 网易云音乐
│   │   ├── tencent/      # QQ 音乐
│   │   └── ...
│   ├── service/           # 服务
│   │   └── api.js        # API 处理
│   └── util.js           # 工具函数
├── app.js                 # Hono 应用
├── vercel.json            # Vercel 配置
├── package.json           # 项目配置
└── cache/                 # 缓存目录
```

## 常见问题

### QQ 音乐无法获取播放链接

确保部署环境有网络访问权限，并且能连接到 QQ 音乐 API。国外部署需要设置 `OVERSEAS=1`。

### 歌词显示为空

某些歌曲可能没有歌词数据，这是正常情况。

### 速度慢

- Vercel 冷启动可能比较慢
- 考虑使用自定义域名而不是 vercel.app 提供的域名
- 确保客户端在同一地区或使用 CDN 加速

## 更新日志

### V1-fix Node.js 版本

- ✨ 转换为 Node.js 实现
- ✨ 支持 Vercel 部署
- ✨ 保留原有功能特性
- 🔄 基于 V1 架构重构

---

**需要帮助？** 查看 [Issues](https://github.com/YOUR_USERNAME/YOUR_REPO/issues) 或提交新问题。
