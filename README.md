# 世界啤酒文化大屏轮播

16:9 电视大屏轮播，电影 Poster 风，图片+视频混合轮播。可视化后台改文字、换图、传 Logo。

## 目录结构

```
beer-screen/
├── index.html              # 前台大屏轮播
├── admin/
│   ├── index.html          # 后台入口（Decap CMS）
│   └── config.yml          # 后台配置
├── content/
│   ├── slides.json         # 轮播素材列表
│   └── settings.json       # Logo、角标等全局设置
├── assets/
│   ├── images/             # 图片素材
│   ├── videos/             # 视频素材
│   └── uploads/            # 后台上传的素材自动存这里
└── vercel.json             # 缓存策略
```

## 部署三步走

### 1. 建 GitHub 仓库
在 GitHub 新建一个仓库（比如叫 `beer-screen`），把这个文件夹里所有内容 push 上去。

### 2. 改后台配置
打开 `admin/config.yml`，把这两行改成你的仓库信息：
```
repo: 你的GitHub用户名/beer-screen
```
部署后把 `site_url` 改成你的 Vercel 域名。

### 3. Vercel 导入
- 打开 vercel.com，用 GitHub 登录
- 点 "Add New Project" → 选中你的 `beer-screen` 仓库 → 直接 Deploy（框架选 Other）
- 部署完拿到一个域名，比如 `beer-screen.vercel.app`

### 4. 后台登录
浏览器打开 `https://你的域名/admin`，点 Login with GitHub，授权后就能：
- 改每条轮播的标题、副标题、标签
- 上传新图片/视频、换图
- 传店铺 Logo、开关角落显示
- 调顺序、启停某条素材

保存后自动 commit 到 GitHub，Vercel 自动重新部署，大屏 20 秒内自动切到新内容。

### 5. Cloudflare 缓存（可选）
Vercel 自带 CDN。要再叠 Cloudflare：
- 域名托管到 Cloudflare
- Cloudflare 后台 → Caching → 配置：
  - `/content/*` 缓存 TTL 30 秒
  - `/assets/*` 缓存 1 年
  - 开启 "Browser Cache TTL" 和 "Always Online"
- 改完素材后在 Cloudflare 后台 Purge Everything 一次（或等 30 秒自动过期）

## 电视上播放
把大屏地址 `https://你的域名` 在电视浏览器打开，全屏即可。自动循环播放，视频静音播完自动切下一条。

## 本地预览
直接双击 `index.html` 可能因为 fetch JSON 被浏览器拦。用本地静态服务器：
```
npx serve .
```
然后浏览器开 `http://localhost:3000`。
