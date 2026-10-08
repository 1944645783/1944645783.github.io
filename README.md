# 博客发布完整流程

> 本博客：Hexo 8 + Butterfly 5.7 + GitHub Pages（自动部署）
> 本地目录：`D:\Blog\blog-site`

---

## 一、日常写作流程

```powershell
# 1. 先本地启动预览（可选）
npx hexo server
# 浏览器打开 http://localhost:4000

# 2. 新建文章
npx hexo new post "文章标题"

# 3. 编辑 source/_posts/文章标题.md

# 4. 发布（三行命令）
git add -A
git commit -m "post: 文章标题"
git push
```

推送后 GitHub Actions 自动构建部署，约 1-2 分钟后网站更新。

> 可以用 `gh run watch` 实时查看部署进度。

---

## 二、首次部署步骤（一次性）

### 1. 登录 GitHub CLI

```powershell
gh auth login
# 选择: GitHub.com → HTTPS → Login with a web browser
# 输入显示的验证码，浏览器授权
```

### 2. 创建仓库并推送

```powershell
# 方案A（推荐）: 仓库名用 用户名.github.io，网站地址为 https://用户名.github.io
gh repo create 用户名.github.io --public --source=. --remote=origin --push

# 方案B: 仓库名 blog，网站地址为 https://用户名.github.io/blog
# 需要修改 _config.yml: url: https://用户名.github.io/blog 且 root: /blog/
gh repo create blog --public --source=. --remote=origin --push
```

### 3. 启用 GitHub Pages

```powershell
gh api -X POST repos/用户名/仓库名/pages -f build_type=workflow
```

或网页操作：仓库 → Settings → Pages → Source 选 **GitHub Actions**

### 4. 等待自动部署完成

```powershell
gh run list        # 查看运行状态
gh run watch       # 实时观察
```

---

## 三、绑定自定义域名（后期）

### 域名购买建议

| 注册商 | 特点 | 价格参考 (.com) |
|--------|------|----------------|
| 阿里云 | 国内最方便，支付宝付款 | ¥60-80/年 首年 |
| 腾讯云 | 同上，经常有活动 | ¥50-70/年 首年 |
| Cloudflare | 按成本价，无溢价续费 | ~$10/年 |
| Namesilo | 便宜实惠，免费隐私保护 | ~$10/年 |

**建议**：先买 `.com`（首年便宜的域名，续费价要看清）。`.top`/`.xyz` 首年几块钱但续费贵。

### DNS 配置（以阿里云为例）

1. 域名解析 → 添加记录：

| 记录类型 | 主机记录 | 记录值 |
|----------|----------|--------|
| CNAME | www | 用户名.github.io |

2. 仓库 → Settings → Pages → Custom domain 填入 `www.你的域名.com`
3. 勾选 **Enforce HTTPS**

> 注意：GitHub Pages 绑国内域名**不需要备案**。
> 如果以后迁到国内服务器才需要备案。

---

## 四、常用命令速查

| 命令 | 作用 |
|------|------|
| `npx hexo clean` | 清空缓存 |
| `npx hexo generate` | 生成静态文件到 public/ |
| `npx hexo server` | 本地预览（默认 4000 端口） |
| `npx hexo new post "标题"` | 新建文章 |
| `npx hexo new page "名称"` | 新建页面 |
| `npx hexo new draft "草稿"` | 新建草稿 |

## 五、文件说明

| 文件/目录 | 作用 |
|-----------|------|
| `_config.yml` | 站点配置（标题、作者、URL） |
| `_config.butterfly.yml` | 主题配置（菜单、样式、功能） |
| `source/_posts/` | 文章目录 |
| `source/about/` 等 | 独立页面 |
| `.github/workflows/pages.yml` | 自动部署配置 |
| `public/` | 生成的静态文件（不提交） |

## 六、进阶功能（Butterfly 支持）

在 `_config.butterfly.yml` 中启用：

- **本地搜索**：安装 `hexo-generator-searchdb`，`search.use: local_search`
- **评论系统**：Twikoo / Waline / Giscus
- **访问统计**：不蒜子（已内置，`busuanzi` 配置）
- **深色模式**：`display_mode: dark` 或设置自动切换
- **图片灯箱**：`fancybox` 相关配置

主题文档：https://butterfly.js.org/
