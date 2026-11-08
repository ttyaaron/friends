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
3. 选择 [添加友链模板](https://github.com/ttyaaron/friends/issues/new?template=friend-link.md)，保留 `json` 代码块标记并填写：

```json
{
  "title": "网站名称",
  "url": "https://example.com/",
  "description": "网站描述",
  "icon": "https://example.com/avatar.png",
  "feed": "https://example.com/atom.xml"
}
```

请使用 JSON 代码块，不使用 YAML。`icon` 是头像字段，`avatar` 不能代替它。

提交后默认带「审核中」标签，管理员确认后移除该标签。**审核通过后仍须保持 Issue 开启**；关闭表示移除友链。编辑已经通过的 Issue 会触发数据重新生成。生成和 CDN 缓存更新不是即时完成的。

### 方式二：编辑本仓库（适合管理员）

直接编辑仓库中的 Issues，GitHub Actions 会自动处理。

## 自动化工作流

### 1. Links Checker（友链状态检查器）
- 触发时机：Issue 新增、编辑、状态或标签变化、手动触发
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
4. 手动运行一次 Workflows 测试是否正常工作
5. 开始通过 Issues 添加友链

## 常见问题

**Q: 如何手动触发 Workflow？**
A: 进入 Actions 页面，选择对应的 Workflow，点击 "Run workflow" 按钮。

**Q: Workflow 成功，但新友链没有出现？**
A: 先检查申请 Issue 是否仍开启、是否已移除「审核中」、正文是否包含合法的 JSON 代码块。再检查 `output/v2/data.json` 中有没有该条的 `issue_number`。输出有条目而网页没有时，再排查 CDN 或浏览器缓存。README 中的 YAML 示例已修正。

**Q: Workflow 显示 No changes to commit 是失败吗？**
A: 不一定；它也可能表示新数据与已有输出相同。以生成文件的内容和 Issue 状态为准。

**Q: 要不要关闭已经通过的申请？**
A: 不要。解析器只读取开启的 Issue，关闭就是下架。

**Q: 如何修改友链检查的频率？**
A: 编辑 `.github/workflows/feed-parser.yml` 中的 `cron` 表达式。

## 参考资料

- [xaoxuu/links-checker](https://github.com/xaoxuu/links-checker)
- [xaoxuu/feed-posts-parser](https://github.com/xaoxuu/feed-posts-parser)
- [xaoxuu/issues2json](https://github.com/xaoxuu/issues2json)
