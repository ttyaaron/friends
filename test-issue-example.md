# Test Friend Link Issue Example

Copy this content when creating a test issue on GitHub:

```yaml
---
title: xaoxuu's blog
url: https://xaoxuu.com
avatar: https://github.com/xaoxuu.png
description: 一个测试友链
keywords:
  - 博客
  - 技术
feed: https://xaoxuu.com/atom.xml
---
```

## Steps to test:

1. Go to your repository's Issues page
2. Click "New Issue"
3. Select the "添加友链" template (or create a blank issue)
4. Paste the content above
5. Submit the issue
6. Go to Actions tab and manually run "Reachability Checker" workflow
7. Check the logs to see if it generates the JSON
8. After successful run, check if the `output` branch was created
9. View the file at: `output` branch → `v2/data.json`

## Common Issues to Check:

1. **Repository Settings** → Actions → General:
   - Workflow permissions: "Read and write permissions" ✓
   - Allow GitHub Actions to create and approve pull requests ✓

2. **After running the workflow**, check the logs for any errors

3. **If the workflow succeeds** but you see "No changes to commit", it means the action ran but found no issues to process
