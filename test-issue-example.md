# Test Friend Link Issue Example

Copy this content when creating a test issue on GitHub:

**IMPORTANT: Use JSON format, not YAML!**

```json
{
  "title": "xaoxuu's blog",
  "url": "https://xaoxuu.com/",
  "description": "一个测试友链",
  "icon": "https://github.com/xaoxuu.png",
  "snapshot": "",
  "feed": "https://xaoxuu.com/atom.xml"
}
```

## Steps to test:

1. Go to your repository's Issues page
2. Click "New Issue"
3. Select the "添加友链" template (or create a blank issue)
4. Paste the content above
5. Submit the issue
6. Keep the issue OPEN and, as the administrator, remove the `审核中` label after review.
7. The label change automatically triggers "Reachability Checker"; it can also be run manually.
8. Check the logs to see if it generates the JSON
9. After successful run, check if the `output` branch was created
10. View the file at: `output` branch → `v2/data.json`

## Common Issues to Check:

1. **Repository Settings** → Actions → General:
   - Workflow permissions: "Read and write permissions" ✓

2. **After running the workflow**, check the logs for any errors

3. **If the workflow succeeds** but you see "No changes to commit", the generated data may simply be unchanged. Check the output JSON, open state, JSON syntax and review labels before diagnosing a failure.
4. Closing an approved issue removes the friend from output; editing an open approved issue regenerates its data.
