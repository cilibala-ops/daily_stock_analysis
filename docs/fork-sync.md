# Fork 自动同步 / Automatic upstream sync

此 Fork 的 `.github/workflows/sync-upstream.yml` 每天北京时间 **09:15**
（UTC 01:15）通过 GitHub 原生 `merge-upstream` 接口同步父仓库的默认分支。
GitHub 排队可能导致延迟。股票分析工作流在周一到周五北京时间 **09:00**
（UTC 01:00，`0 1 * * 1-5`）启动，使用运行时默认分支上的代码；邮件在分析完成后发送。
自动同步计划在 09:15 运行，因此 09:00 的分析不保证包含当天同步的更新。

- 默认使用当前工作流的 `GITHUB_TOKEN`，权限为 `contents: write`。当上游修改 `.github/workflows/` 文件时，此令牌不足以完成合并，需要配置专用的 Repository Secret `UPSTREAM_SYNC_TOKEN`。
- 建议创建 fine-grained PAT，只选择当前 Fork 仓库，授予 Contents: Read and write、Workflows: Read and write，设置到期时间；不要授予其他仓库或账号管理权限。将生成的令牌保存到 `UPSTREAM_SYNC_TOKEN`，到期后需替换。不要把令牌提交进代码或聊天记录。
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
Stock analysis starts Monday through Friday at **01:00 UTC (09:00 Asia/Shanghai)**
(`0 1 * * 1-5`), and emails are sent after analysis completes. Since upstream
sync is scheduled for 09:15, the 09:00 analysis may run before that day's sync.

The workflow defaults to its built-in `GITHUB_TOKEN` with `contents: write`.
If upstream changes workflow files, configure the Repository Secret
`UPSTREAM_SYNC_TOKEN` with a fine-grained PAT restricted to this fork, granting
Contents and Workflows read/write permissions and an expiration date. Replace
it when it expires. Never commit the token. Stock-analysis secrets are not used,
and Repository Secrets and Variables are preserved. Local commits are retained, and conflicts or permission
errors fail the run without a force reset. Incoming upstream changes are not
reviewed for compatibility and may require additional configuration.

Run it manually from **Actions → 自动同步上游 → Run workflow**. Use **Disable
workflow** to pause it and **Enable workflow** to resume it, including after an
inactivity-related schedule disablement by GitHub.
