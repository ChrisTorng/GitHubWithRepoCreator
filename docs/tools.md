# 當次工作階段提供的 48 個工具

依 2026-10-16 使用者提供的工具列舉紀錄整理。這是當次工作階段清單，不是永遠固定的 API 合約，也不是每個工具都已測試。各工具的即時輸入 schema 與執行所需 scope，請以你的工作階段為準。

| 類別 | 工具 |
|---|---|
| Repo（3） | `create_repository`、`fork_repository`、`search_repositories` |
| 分支／檔案／Commit（10） | `create_branch`、`list_branches`、`create_or_update_file`、`delete_file`、`get_file_contents`、`push_files`、`get_commit`、`list_commits`、`search_commits`、`search_code` |
| PR／Review（13） | `create_pull_request`、`create_pull_request_with_copilot`、`list_pull_requests`、`search_pull_requests`、`pull_request_read`、`update_pull_request`、`update_pull_request_branch`、`merge_pull_request`、`pull_request_review_write`、`add_comment_to_pending_review`、`add_reply_to_pull_request_comment`、`request_copilot_review`、`get_copilot_job_status` |
| Issue（9） | `issue_read`、`issue_write`、`list_issues`、`search_issues`、`list_issue_fields`、`list_issue_types`、`sub_issue_write`、`add_issue_comment`、`update_issue_comment` |
| Release／Tag（4） | `list_releases`、`get_latest_release`、`get_release_by_tag`、`list_tags` |
| 使用者／組織（5） | `get_me`、`get_teams`、`get_team_members`、`search_users`、`list_repository_collaborators` |
| 其他（4） | `get_label`、`get_tag`、`assign_copilot_to_issue`、`run_secret_scanning` |

## 建庫參數

當次看到的介面如下；注意 `autoInit` 與 REST API 的 `auto_init` 不同。

```typescript
create_repository(args: {
  name: string;
  description?: string;
  organization?: string;
  private?: boolean; // 當次工具預設 true
  autoInit?: boolean;
}): Promise<Record<string, any>>;
```

一般個人帳號建庫省略 organization。請明確指定 private，避免不同工具版本預設值不一致。此專案沒有驗證組織建庫、Copilot 功能或其他付費／政策限制。
