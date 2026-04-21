# Decap CMS 本地开发与调试

## 快速启动

```bash
npm ci
npx decap-server
npm run server
```

默认情况下：

- 博客站点：`http://localhost:4000`
- CMS 后台：`http://localhost:4000/admin`
- 本地后端：`http://localhost:8081`

`source/admin/config.yml` 中已启用 `local_backend: true`，本地调试会优先走本地后端。

## 本地发布流程

1. 打开 `/admin`。
2. 点击「新建文章」并填写内容。
3. 点击「发布」后会直接写入 `source/_posts`。
4. Hexo 本地服务会自动刷新页面。

## 常见问题

### 1) `/admin` 空白页

- 检查网络是否可访问 `unpkg.com`（Decap CMS 脚本来源）。
- 打开浏览器控制台查看脚本加载错误。
- 如需避免公共 CDN，可将 `source/admin/index.html` 中脚本改为自托管静态文件地址。

### 2) 无法登录 GitHub

- 检查 `source/admin/config.yml` 中 `base_url` 是否已替换为你的 OAuth 代理服务地址。
- 确认 OAuth 回调地址与配置一致。

### 3) 图片上传失败

- 检查仓库权限（CMS 是否有写入权限）。
- 检查 `media_folder` 路径是否存在并可写。
