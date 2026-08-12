+++
date = '2026-08-12T18:00:00+08:00'
draft = false
title = '如何在博客中归档和阅读翻译论文'
+++

## 功能说明

博客提供了独立的[论文归档](/papers/)入口，可以归档 Markdown 或完整 HTML 格式的翻译论文。

- Markdown 论文由 Hugo 渲染，页面样式与普通博客文章一致。
- HTML 论文按原样发布，适合保留自定义排版、内嵌图片和交互功能。

## 上传 Markdown 论文

将翻译好的 Markdown 文件放入：

```text
content/papers/
```

例如：

```text
content/papers/lean-introduction.md
```

文件开头需要包含论文元数据：

```toml
+++
title = '论文标题'
date = '2026-08-12T18:00:00+08:00'
draft = false
authors = ['作者一', '作者二']
paperURL = 'https://arxiv.org/abs/论文编号'
+++
```

在元数据后直接编写论文正文。发布后，论文会自动出现在论文归档页中，点击标题即可进入 Hugo 渲染的阅读页面。

## 上传 HTML 论文

完整 HTML 文件应当放入：

```text
static/papers/
```

建议使用只包含英文字母、数字和连字符的文件名，例如：

```text
static/papers/lean-introduction.html
```

随后在 `content/papers/` 中创建一个对应的 Markdown 元数据文件：

```text
content/papers/lean-introduction.md
```

文件内容如下：

```toml
+++
title = '论文标题'
date = '2026-08-12T18:00:00+08:00'
draft = false
authors = ['作者一', '作者二']
paperURL = 'https://arxiv.org/abs/论文编号'
externalURL = '/papers/lean-introduction.html'
+++
```

`externalURL` 必须与 `static/papers/` 中的 HTML 文件名对应。该元数据文件负责让论文出现在归档页中，点击标题后会直接打开完整 HTML 页面。

如果 HTML 引用了单独的图片、CSS 或 JavaScript 文件，也需要将这些资源一并放入 `static/papers/`，并保持 HTML 中的相对路径不变。使用内嵌样式、脚本和图片的自包含 HTML 最容易归档。

## 元数据字段

- `title`：归档页显示的论文标题。
- `date`：归档日期，用于论文排序。
- `draft`：设置为 `false` 才会正式发布。
- `authors`：原论文作者列表。
- `paperURL`：原论文页面，例如 arXiv 地址。
- `externalURL`：仅 HTML 论文需要，指向公开阅读地址。

## 本地预览

在博客项目目录运行：

```powershell
hugo server
```

然后访问：

```text
http://localhost:1313/papers/
```

检查以下内容：

1. 新论文是否出现在归档页中。
2. 标题、作者和日期是否正确。
3. 点击标题后是否能打开论文正文。
4. HTML 论文中的图片、公式和交互功能是否正常。

## 提交和发布

确认本地预览正常后执行：

```powershell
git add content/papers static/papers
git commit -m "docs: add translated paper"
git push origin main
```

如果只添加了 Markdown 论文，可以省略不存在的 `static/papers` 变更。推送到 `main` 后，GitHub Actions 会自动构建并部署博客。

部署完成后的归档页地址为：

```text
https://zhenquin.github.io/papers/
```

HTML 论文的地址形式为：

```text
https://zhenquin.github.io/papers/文件名.html
```

## 常见问题

### 论文没有出现在归档页

检查元数据中的 `draft` 是否为 `false`，并确认文件位于 `content/papers/`。

### HTML 页面显示 404

检查 HTML 是否位于 `static/papers/`，以及 `externalURL` 的文件名和大小写是否完全一致。

### HTML 中的图片无法显示

自包含 HTML 通常不会出现此问题。如果使用外部资源，请确认资源已经提交到仓库，并检查 HTML 中的相对路径。

### 本地可以阅读，线上无法阅读

文件名应尽量使用英文、数字和连字符，避免不同系统对空格、中文字符或大小写的处理差异。同时检查 GitHub Actions 的构建结果是否成功。
