# 茅盾文学奖网站设计文档

**日期**: 2026-04-06
**版本**: 1.0

## 项目概述

创建一个展示历年茅盾文学奖作家和作品的静态网站，采用现代卡片式设计风格，使用 Hugo 生成静态页面，部署到 GitHub Pages。

## 目标

- 公开展示茅盾文学奖获奖作品和作家信息
- 提供清晰、优雅的用户界面
- 完全免费托管和部署

## 技术栈

| 组件 | 选择 | 原因 |
|------|------|------|
| 静态网站生成器 | Hugo | 构建速度快，中文文档完善，主题丰富 |
| 样式 | 原生 CSS | 轻量，无依赖，完全控制 |
| 托管 | GitHub Pages | 免费托管，支持自定义域名 |
| 数据格式 | YAML | 简洁易读，Hugo 原生支持 |

## 项目结构

```
maodun-award/
├── content/
│   └── awards/           # 作品详情页（Markdown 格式）
│       ├── 2019/
│       └── 2023/
├── data/
│   └── awards.yaml       # 所有奖项数据
├── layouts/
│   ├── _default/
│   │   └── single.html   # 详情页模板
│   ├── partials/
│   │   ├── header.html   # 头部组件
│   │   ├── footer.html   # 底部组件
│   │   └── card.html     # 卡片组件
│   └── index.html        # 首页
├── static/
│   ├── css/
│   │   └── style.css     # 主样式文件
│   └── images/           # 作品封面图
├── config.toml           # Hugo 配置
└── README.md
```

## 数据结构

### awards.yaml

```yaml
awards:
  - year: 2023
    award_name: "第十一届茅盾文学奖"
    works:
      - title: "雪山大地"
        author: "杨志军"
        author_bio: "出生于青海，当代作家，著有长篇小说《环湖崩溃》等。"
        publisher: "作家出版社"
        year_published: "2022"
        description: "小说以改革开放以来青海藏区的历史变迁为背景..."
        cover_image: "/images/2023-xueshandadi.jpg"
        slug: "xueshandadi"
```

### 字段说明

| 字段 | 类型 | 必填 | 说明 |
|------|------|------|------|
| year | int | 是 | 获奖年份 |
| award_name | string | 是 | 届数名称 |
| works | array | 是 | 作品列表 |
| works.title | string | 是 | 作品名称 |
| works.author | string | 是 | 作者姓名 |
| works.author_bio | string | 是 | 作家简介 |
| works.publisher | string | 是 | 出版社 |
| works.year_published | string | 是 | 作品出版年份 |
| works.description | string | 是 | 作品简介 |
| works.cover_image | string | 否 | 封面图片路径 |
| works.slug | string | 是 | URL 友好标识符 |

## 设计规范

### 配色方案

```
主色（深红）：  #8B1A1A - 呼应"茅盾"和文学庄重感
背景（浅米）：  #F5F1E8 - 纸张质感
文字（深灰）：  #2C2C2C
强调（金色）：  #C9A961 - 获奖荣誉感
卡片背景：    #FFFFFF
边框：        #E0D8C8
```

### 字体

```css
font-family: "PingFang SC", "Microsoft YaHei", "Helvetica Neue", sans-serif;
```

### 响应式断点

- 移动端：< 768px（单列）
- 平板：768px - 1024px（双列）
- 桌面：> 1024px（三列）

## 页面设计

### 首页 (index.html)

**结构**：
1. 头部：网站标题 "茅盾文学奖" + 副标题
2. 主区域：卡片网格
3. 底部：版权信息

**卡片组件**：
- 封面图（16:9 比例）
- 书名（加粗，大字号）
- 作者（中等字号）
- 年份标签（小字号，金色背景）

**交互**：
- hover：卡片上移 4px + 阴影加深
- 点击：跳转到详情页

### 详情页 (single.html)

**结构**：
1. 返回首页链接
2. 左侧：封面大图
3. 右侧：
   - 书名
   - 作者
   - 作家简介
   - 出版社、出版年份
   - 作品简介

## 初始内容

### 第十一届（2023）

1. 《雪山大地》- 杨志军
2. 《宝水》- 乔叶
3. 《本巴》- 刘亮程
4. 《千里江山图》- 孙甘露
5. 《回响》- 东西

### 第十届（2019）

1. 《人世间》- 梁晓声
2. 《牵风记》- 徐怀中
3. 《北上》- 徐则臣
4. 《主角》- 陈彦
5. 《应物兄》- 李洱

## 部署配置

### GitHub Actions 工作流

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
      - name: Build
        run: hugo --minify
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

## 成功标准

1. 首页展示所有作品卡片
2. 点击卡片进入详情页，显示完整信息
3. 响应式设计在移动端正常显示
4. 成功部署到 GitHub Pages 可公开访问

---

# 实现计划

> **For agentic workers:** 使用 superpowers:subagent-driven-development 或 superpowers:executing-plans 来执行此计划。

## Task 1: 初始化 Hugo 项目

- [ ] 创建 `config.toml`
- [ ] 运行 `hugo config` 验证
- [ ] 初始化 Git 并提交

```toml
baseURL = "https://[username].github.io/maodun-award/"
languageCode = "zh-cn"
title = "茅盾文学奖"
theme = ""

[params]
  description = "历年茅盾文学奖获奖作品与作家介绍"
```

## Task 2: 创建数据文件 `data/awards.yaml`

包含第十届（2019）和第十一届（2023）共10部作品的完整信息。

## Task 3: 创建样式文件 `static/css/style.css`

实现响应式卡片布局，配色：深红#8B1A1A、浅米#F5F1E8、金色#C9A961。

## Task 4: 创建模板文件

- `layouts/partials/header.html` - 页头
- `layouts/partials/footer.html` - 页脚
- `layouts/partials/card.html` - 卡片组件
- `layouts/index.html` - 首页
- `layouts/_default/single.html` - 详情页

## Task 5: 创建内容页面

在 `content/work/` 目录创建10个作品详情页 Markdown 文件。

## Task 6: 配置 GitHub Actions

创建 `.github/workflows/deploy.yml` 自动部署到 GitHub Pages。

## Task 7: 本地测试

运行 `hugo server -D` 验证功能。

## Task 8: 推送到 GitHub

创建仓库、推送代码、启用 GitHub Pages。

---

# GitHub 部署指南

本指南将帮助你将茅盾文学奖网站部署到 GitHub Pages，实现免费托管和自动部署。

## 前置准备

在开始之前，请确保你已经：

- [ ] 拥有 GitHub 账户（如没有，请访问 https://github.com 注册）
- [ ] 安装了 Git（如没有，请访问 https://git-scm.com/downloads 下载）
- [ ] 本地项目已完成开发并通过测试

## 部署步骤

### 第一步：创建 GitHub 仓库

1. 访问 https://github.com/new
2. 填写仓库信息：
   - **Repository name**: `maodun-award`
   - **Description**: 茅盾文学奖获奖作品展示网站
   - **Public/Private**: 选择 **Public**（GitHub Pages 免费版需要公开仓库）
   - **Initialize this repository**: ❌ 不要勾选任何选项（已有本地仓库）
3. 点击 "Create repository" 按钮

### 第二步：添加远程仓库并推送代码

在项目根目录执行以下命令：

```bash
# 添加远程仓库（请替换 [YOUR_USERNAME] 为你的 GitHub 用户名）
git remote add origin https://github.com/[YOUR_USERNAME]/maodun-award.git

# 将主分支重命名为 main（如果不是的话）
git branch -M main

# 推送代码到 GitHub
git push -u origin main
```

**示例**：
```bash
git remote add origin https://github.com/zhangsan/maodun-award.git
git branch -M main
git push -u origin main
```

### 第三步：验证 GitHub Actions 工作流

推送代码后，GitHub Actions 会自动触发部署流程：

1. 访问你的仓库页面
2. 点击上方的 "Actions" 标签
3. 查看 "Deploy to GitHub Pages" 工作流的运行状态
4. 等待工作流完成（通常需要 1-3 分钟）
5. 如果显示绿色✅，表示部署成功

**如果工作流失败**：
- 检查 `.github/workflows/deploy.yml` 文件是否存在
- 确认仓库是 Public（GitHub Pages 免费版要求）
- 查看 Actions 日志获取详细错误信息

### 第四步：启用 GitHub Pages

1. 在仓库页面，点击 "Settings" 标签
2. 在左侧菜单中找到 "Pages"
3. 在 "Build and deployment" 部分：
   - **Source**: 选择 "GitHub Actions"（不是 "Deploy from a branch"）
   - 保存设置

**注意**：由于我们已经配置了 GitHub Actions 工作流，这里选择 "GitHub Actions" 作为源。

### 第五步：访问你的网站

部署完成后，你的网站将可以通过以下地址访问：

```
https://[YOUR_USERNAME].github.io/maodun-award/
```

**示例**：
```
https://zhangsan.github.io/maodun-award/
```

首次访问可能需要等待 1-2 分钟，如果看到 404 错误，请稍等片刻再刷新。

## 本地开发

如果你想在本地进行开发和测试，需要安装 Hugo：

### 安装 Hugo

**Windows**：
```bash
# 使用 Chocolatey
choco install hugo-extended

# 或从官网下载安装包
# 访问：https://gohugo.io/installation/windows/
```

**macOS**：
```bash
brew install hugo
```

**Linux**：
```bash
# Debian/Ubuntu
sudo apt-get install hugo

# 或从官网下载
# 访问：https://gohugo.io/installation/linux/
```

### 验证安装

```bash
hugo version
```

应该显示 Hugo 的版本信息。

### 本地运行

```bash
# 启动开发服务器（包含草稿页面）
hugo server -D

# 启动开发服务器（不包含草稿页面）
hugo server
```

然后访问 http://localhost:1313 查看网站。

### 本地构建

```bash
# 构建静态网站到 public/ 目录
hugo

# 构建并压缩资源
hugo --minify
```

## 更新网站

当你需要更新网站内容时：

1. 修改本地文件（内容、样式、模板等）
2. 本地测试：`hugo server -D`
3. 提交更改：
   ```bash
   git add .
   git commit -m "描述你的更改"
   git push
   ```
4. GitHub Actions 会自动重新部署
5. 等待 1-3 分钟后访问网站查看更新

## 常见问题

### Q: 推送代码时提示身份验证错误？
**A**: 需要设置 Git 凭据或使用 SSH 密钥。推荐使用 GitHub 的 Personal Access Token：
1. 访问 GitHub Settings → Developer settings → Personal access tokens
2. 生成新 token（勾选 `repo` 权限）
3. 使用 token 作为密码推送

### Q: GitHub Pages 显示 404？
**A**: 检查以下几点：
- 仓库是否设置为 Public
- GitHub Actions 工作流是否成功运行
- 等待 2-3 分钟让 DNS 传播
- 确认 URL 格式正确：`https://[username].github.io/maodun-award/`

### Q: 网站样式显示不正常？
**A**: 确认 `static/css/style.css` 文件存在，并且路径在 HTML 中正确引用。

### Q: 图片无法显示？
**A**: 
- 确保图片文件在 `static/images/` 目录下
- 检查文件名大小写是否匹配（Linux 区分大小写）
- 确认图片路径以 `/images/` 开头（不是相对路径）

### Q: 想要使用自定义域名？
**A**:
1. 在 `static/` 目录创建 `CNAME` 文件
2. 文件内容填写你的域名（如 `www.example.com`）
3. 在域名提供商处添加 DNS 记录
4. 在 GitHub Pages 设置中配置自定义域名

## 进阶配置

### 添加 Google Analytics

在 `layouts/partials/footer.html` 中添加 Google Analytics 跟踪代码。

### 启用评论功能

可以考虑集成 Disqus、Gitalk 或 Giscus 等评论系统。

### 添加搜索功能

Hugo 支持通过 lunr.js 或 Algolia 添加站内搜索功能。

## 维护建议

- **定期更新 Hugo 版本**：GitHub Actions 会自动使用最新版本的 Hugo
- **备份数据**：定期备份 `data/awards.yaml` 和 `content/` 目录
- **监控 Actions**：定期检查 GitHub Actions 运行状态
- **优化性能**：使用 `hugo --minify` 压缩资源，优化图片大小

## 参考资源

- [Hugo 官方文档](https://gohugo.io/documentation/)
- [GitHub Pages 文档](https://docs.github.com/en/pages)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [Hugo 主题列表](https://themes.gohugo.io/)

---

祝你部署顺利！如有问题，可以参考上述资源或查看 GitHub Issues。
