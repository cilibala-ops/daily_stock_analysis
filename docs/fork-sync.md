# Fork 自动同步 / Automatic upstream sync

此 Fork 的 `.github/workflows/sync-upstream.yml` 每天北京时间 **09:15**
（UTC 01:15）通过 GitHub 原生 `merge-upstream` 接口同步父仓库的默认分支。
GitHub 排队可能导致延迟；18:00 的股票分析继续使用默认分支上的代码。

- 无须额外 API Key 或个人访问令牌，使用当前工作流的 `GITHUB_TOKEN`，权限为 `contents: write`。
- 仓库 Secrets / Variables 中的股票列表、邮箱和模型配置不受普通代码合并影响。
- 保留此 Fork 的独立提交。遇到合并冲突或权限错误时，该次同步失败并保留当前分支，不强制重置或覆盖。
- 自动同步会接收上游代码和工作流变更；这不是上游更新的兼容性审查。如果更新引入新必填项，需另外配置。
- 在 **Actions → 自动同步上游 → Run workflow** 可手动同步；日志会显示 GitHub 返回的同步结果。
- 暂停：在该工作流页面选择 **Disable workflow**。恢复时选择 **Enable workflow**。
- 若 GitHub 因长期无仓库活动而停用定时工作流，需要重新启用。

## English

This fork runs `.github/workflows/sync-upstream.yml` daily at **01:15 UTC**
(09:15 Asia/Shanghai). It uses GitHub's native `merge-upstream` endpoint to sync
the fork's default branch with its parent. GitHub may delay scheduled runs.

The workflow uses only its built-in `GITHUB_TOKEN` with `contents: write`; no
personal token or stock-analysis secrets are required. Repository Secrets and
Variables are preserved. Local commits are retained, and conflicts or permission
errors fail the run without a force reset. Incoming upstream changes are not
reviewed for compatibility and may require additional configuration.

Run it manually from **Actions → 自动同步上游 → Run workflow**. Use **Disable
workflow** to pause it and **Enable workflow** to resume it, including after an
inactivity-related schedule disablement by GitHub.
