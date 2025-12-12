# 友链管理仓库

这是一个用于管理博客友链的 GitHub 仓库，使用 GitHub Issues 和 Workflows 实现动态友链功能。

## 功能特性

- ✅ 自动检查友链可达性
- ✅ 自动解析友链的 RSS 订阅，展示最新文章
- ✅ 通过 GitHub Issues 管理友链
- ✅ 自动生成 JSON 数据供博客调用

## 如何添加友链？

### 方式一：提交 Issue（推荐）

1. 点击 [Issues](../../issues) 页面
2. 点击 "New Issue"
3. 按照以下格式填写 Issue：

```yaml
---
title: 网站名称
url: https://example.com
avatar: https://example.com/avatar.png
description: 网站描述
keywords:
  - 关键词1
  - 关键词2
feed: https://example.com/atom.xml  # 可选，用于订阅文章更新
---
```

### 方式二：编辑本仓库（适合管理员）

直接编辑仓库中的 Issues，GitHub Actions 会自动处理。

## 自动化工作流

### 1. Links Checker（友链状态检查器）
- 触发时机：Issue 状态变化、手动触发
- 功能：检查友链是否可访问，自动标记失联网站
- 配置文件：`.github/workflows/links-checker.yml`

### 2. Feed Posts Parser（友链文章订阅器）
- 触发时机：每天凌晨 05:00（东八区时间）、手动触发
- 功能：解析友链的 RSS 订阅，获取最新文章
- 配置文件：`.github/workflows/feed-parser.yml`

## 在博客中使用

生成的友链数据会保存在 `output` 分支的 `/v2/data.json` 文件中。

在你的博客友链页面中，使用以下标签调用：

```markdown
{% friends api:https://raw.githubusercontent.com/你的用户名/你的仓库名/output/v2/data.json %}
```

如果想显示友链的最新文章（需要主题支持），添加 `posts:true` 参数：

```markdown
{% friends posts:true api:https://raw.githubusercontent.com/你的用户名/你的仓库名/output/v2/data.json %}
```

## 标签说明

为了更好地管理友链，可以给 Issue 添加以下标签：

- `审核中`：新提交的友链，等待审核
- `白名单`：可信任的友链，不会在前端显示此标签
- `失联`：网站无法访问
- `缺少互动`：长期无互动
- `缺少文章`：网站内容不足
- `风险网站`：存在风险的网站

## 部署步骤

1. 创建一个新的 GitHub 仓库（或使用现有仓库）
2. 将本目录下的所有文件上传到该仓库
3. 在 GitHub 仓库的 Settings → Actions → General 中：
   - 确保 "Actions permissions" 设置为 "Allow all actions and reusable workflows"
   - 在 "Workflow permissions" 中选择 "Read and write permissions"
   - 勾选 "Allow GitHub Actions to create and approve pull requests"
4. 手动运行一次 Workflows 测试是否正常工作
5. 开始通过 Issues 添加友链

## 常见问题

**Q: 如何手动触发 Workflow？**
A: 进入 Actions 页面，选择对应的 Workflow，点击 "Run workflow" 按钮。

**Q: 为什么友链数据没有更新？**
A: 检查 Actions 页面是否有错误，确保 Workflow permissions 配置正确。

**Q: 如何修改友链检查的频率？**
A: 编辑 `.github/workflows/feed-parser.yml` 中的 `cron` 表达式。

## 参考资料

- [xaoxuu/links-checker](https://github.com/xaoxuu/links-checker)
- [xaoxuu/feed-posts-parser](https://github.com/xaoxuu/feed-posts-parser)
- [xaoxuu/issues2json](https://github.com/xaoxuu/issues2json)
