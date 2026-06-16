# Hugo + PaperMod 免费博客方案

这个仓库按 Hugo + PaperMod + GitHub Pages + Cloudflare + Giscus 搭好基础博客。

## 架构

- Hugo：本地写 Markdown，生成静态站点。
- PaperMod：轻量主题，支持归档、搜索、RSS、暗色模式、目录、代码复制。
- GitHub Pages：免费托管 `public` 构建产物，但仓库不提交 `public/`。
- GitHub Actions：push 到 `main` 后自动构建和发布。
- Cloudflare：接管域名 DNS，开启 HTTPS、CDN、缓存和基础防护。
- Giscus：基于 GitHub Discussions 的免费评论系统。

## 首次发布

1. 创建 GitHub 仓库，并把本地仓库推到 GitHub。
2. 在 GitHub 仓库 Settings > Pages 中，把 Source 改成 `GitHub Actions`。
3. 如果使用自定义域名，在 GitHub Pages 的 Custom domain 填入域名，例如 `blog.example.com`。
4. 在 Cloudflare 中添加域名，按提示把域名注册商的 nameserver 改成 Cloudflare。
5. 在 Cloudflare DNS 添加记录：
   - 子域名方案：`CNAME blog YOUR_GITHUB_USERNAME.github.io`
   - 根域名方案：使用 GitHub Pages 文档给出的 A/AAAA 记录，并建议同时配置 `www` CNAME。
6. 在 Cloudflare SSL/TLS 里选择 `Full`，GitHub Pages 证书生效后开启 Always Use HTTPS。

## Giscus 配置

1. GitHub 仓库必须是 public。
2. 在仓库 Settings > General 开启 Discussions。
3. 安装 Giscus GitHub App，并授权这个仓库。
4. 打开 <https://giscus.app/>，填入仓库和 Discussion 分类，复制生成配置中的：
   - `data-repo`
   - `data-repo-id`
   - `data-category`
   - `data-category-id`
5. 写入 `hugo.yaml` 的 `params.giscus`。四个字段未填时，评论脚本不会渲染，构建不会失败。

## 常用命令

```powershell
hugo server -D
hugo --gc --minify --cacheDir "$PWD/.hugo_cache"
```

## 内容组织

- 新文章放在 `content/posts/`。
- 草稿文章 front matter 中设置 `draft: true`。
- 发布前把 `draft` 改成 `false`，提交并 push 到 `main`。

## 需要替换的占位

- `hugo.yaml` 的 `baseURL`
- `hugo.yaml` 的 `title`
- `hugo.yaml` 的 `params.author`
- `hugo.yaml` 的 GitHub social link
- `hugo.yaml` 的 `params.giscus`
