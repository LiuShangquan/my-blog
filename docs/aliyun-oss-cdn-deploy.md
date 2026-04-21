# 阿里云 OSS + CDN 部署指南

## 1. 创建 OSS Bucket

1. 登录阿里云控制台，进入 OSS。
2. 创建 Bucket（建议地域与用户接近，如 `华东1-杭州`）。
3. 读写权限建议设置为 `公共读`（静态站点场景）。
4. 在 Bucket 基础设置中绑定你的自定义域名（可选）。

## 2. 配置跨域（CORS）

在 OSS Bucket 的 CORS 中添加规则：

- 来源（Origin）：`https://你的博客域名`
- 允许方法：`GET, HEAD`
- 允许 Headers：`*`
- 暴露 Headers：`ETag`
- 缓存时间：`600`

> 如果使用 Decap CMS 上传图片，需确保对应域名跨域已放开。

## 3. 配置 CDN 加速

1. 在阿里云 CDN 控制台添加加速域名（建议与博客主域名一致或二级域名）。
2. 回源站点类型选择 OSS Bucket。
3. 开启 HTTPS（上传证书或使用阿里云证书服务）。
4. 根据需要设置缓存规则（HTML 短缓存、静态资源长缓存）。

## 4. 配置 GitHub Actions Secrets

在 GitHub 仓库 `Settings -> Secrets and variables -> Actions` 中添加：

- `ALIYUN_ACCESS_KEY_ID`
- `ALIYUN_ACCESS_KEY_SECRET`
- `ALIYUN_REGION_ID`（例如 `cn-hangzhou`）
- `ALIYUN_OSS_ENDPOINT`（例如 `oss-cn-hangzhou.aliyuncs.com`）
- `ALIYUN_OSS_BUCKET`（你的 bucket 名称）
- `ALIYUN_CDN_OBJECT_PATHS`（刷新路径, 多个用换行分隔, 如 `https://blog.example.com/`）

## 5. 验证自动部署

1. 推送任意 commit 到 `main`（或 `master`）。
2. 查看 `Actions` 页的 `Deploy Hexo to Aliyun OSS` 工作流。
3. 成功后访问域名确认内容已更新。

## 6. 配置 Decap CMS 关键参数

编辑 `/source/admin/config.yml`：

- 将 `backend.base_url` 替换为你的 GitHub OAuth 代理地址
- 将 `site_url`、`display_url` 替换为你的博客正式域名
- 根据默认分支调整 `backend.branch`（`main` 或 `master`）

## 7. 性能优化建议

- OSS 静态资源开启更长缓存时间，HTML 设置短缓存。
- CDN 开启 Brotli/Gzip 压缩。
- 图片建议提前压缩并使用 WebP/AVIF。
- 可在 Hexo 构建阶段增加资源压缩插件后再部署。
