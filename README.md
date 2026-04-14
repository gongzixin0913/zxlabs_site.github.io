# ZX Labs Photography

个人摄影作品集网站。基于纯静态 HTML + 腾讯云 COS 图片存储。

## 项目结构

```
├── index.html      ← 网站页面（一般不需要改）
├── config.json     ← 配置文件（所有内容都改这里）
├── CNAME           ← 自定义域名（已设置为 zxlabs.cn）
└── README.md       ← 本文件
```

## 如何更新照片

### 1. 上传照片到腾讯云 COS

用 COSBrowser 客户端或网页控制台，把照片传到你的存储桶，建议按分类放：

```
/photos/landscape/photo-001.jpg
/photos/portrait/photo-002.jpg
/photos/street/photo-003.jpg
/photos/night/photo-004.jpg
```

### 2. 编辑 config.json

打开 config.json，在 `photos` 数组里加一条新记录：

```json
{
  "file": "/photos/landscape/new-photo.jpg",
  "title": "照片标题",
  "category": "风光",
  "location": "拍摄地点",
  "date": "2025"
}
```

可选属性：
- `"wide": true` → 横向占两列（适合全景/宽幅照片）
- `"tall": true` → 纵向占两行（适合竖拍照片）

### 3. 推送更新

```bash
git add .
git commit -m "add new photos"
git push
```

等 1-2 分钟 GitHub Pages 自动部署完成，网站就更新了。

## 配置说明

### cos_base（重要！）

这是你的腾讯云 COS 存储桶访问地址，格式为：

```
https://存储桶名-APPID.cos.地域.myqcloud.com
```

例如：`https://my-photos-1250088888.cos.ap-shanghai.myqcloud.com`

在 COS 控制台的存储桶概览页可以找到这个地址。

### 图片自动压缩

网站会自动拼接 COS 图片处理参数：
- 画廊缩略图：`?imageMogr2/thumbnail/600x/quality/85`
- 大图查看：`?imageMogr2/thumbnail/1600x/quality/85`
- 原图：直接访问原始文件

你只需要上传原图，不需要手动压缩。

### 修改网站信息

编辑 config.json 中的对应部分：
- `site` → 网站标题、副标题
- `about` → 个人介绍、头像、数据统计
- `contact` → 联系方式
- `categories` → 照片分类标签

## 首次部署步骤

```bash
# 1. 进入项目目录
cd zxlabs-photography

# 2. 初始化 Git
git init
git add .
git commit -m "init: photography portfolio"

# 3. 关联 GitHub 仓库（先在 GitHub 上创建仓库）
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git branch -M main
git push -u origin main

# 4. 去 GitHub 仓库 → Settings → Pages → Source 选 "Deploy from a branch" → main / root
# 5. 去 Settings → Pages → Custom domain 输入 zxlabs.cn，勾选 Enforce HTTPS
# 6. 在腾讯云 DNS 解析中添加 CNAME 记录指向 你的用户名.github.io
```

## 本地预览

直接用浏览器打开 index.html 即可预览（照片需要 COS 配置好后才能加载）。

或者用 VS Code 安装 Live Server 插件，右键 index.html → Open with Live Server。