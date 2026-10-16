# 已嘗試但未成功／未完成的方向

先採用 README 的已驗證路徑。以下是探索紀錄，不是替代安裝指引；「未成功」不等於產品永遠不支援。

| 方向 | 實際結果與限制 |
|---|---|
| 使用 OpenAI 原有 GitHub connector 建庫 | 本次 connector 工具清單沒有 create_repository；不能因此說 GitHub 官方 MCP 也沒有此工具。官方 plugin 公開套件同時包含 App 宣告與 MCP 設定，不能把兩者當成相同後端。 |
| 表單留白、使用 OpenAI 預設 OAuth client | UI 宣稱有預設 client，實際建立時卻得到 Couldn't register an OAuth client；自有 OAuth App 的後續流程成功。錯誤本身不能證明是前端 bug 或特定後端原因。 |
| DCR／CIMD 自動 client 註冊 | GitHub 當次 discovery 未宣告 registration_endpoint 或 CIMD，表單也顯示不可用；不能任填 Registration URL，更不能把 example.com 當端點。 |
| 個人 App 設定逐項關閉工具 | Details、Manage 與省略號選單均未看到開關。較籠統的開發文件寫有開關，但較具體的 Help Center 將工作區 Action control 記載於 Enterprise/Edu 管理員流程；沒有驗證個人帳號可用。 |
| 更改核准模式來只保留建庫 | Allow all／low-risk 等控制何時核准，不是工具白名單。不能用它刪去其他 47 個工具。 |
| header 限制單一工具 | GitHub 官方文件支援 X-MCP-Tools，但本次一般 Chat 自訂 MCP 表單沒有自訂 header 欄位。未成功把它加入已註冊 App 連線。 |
| gh-repo-creator-only 私人套件 | Plugin Creator 接受 plugin.json 與 mcp.json，回讀保留 X-MCP-Tools。UI 顯示 MCP servers，而非已連接的 App；網頁顯示 Open in desktop app，沒有完成 OAuth。一般 Chat 未取得該套件的工具。只證明檔案保存成功，不證明單工具連線成功。 |
| Only 套件齒輪 | 提示 Set up in MCP settings，導向共用 MCP 設定頁；頁面出現 node_repl、code-review、codex_apps，不代表它們是 GitHub Server，也不應為此改動既有系統工具。 |
| 改成 /mcp/x/repos | 官方支援 toolset URL，但 repos 是整組 Repo 工具，不等於只留 create_repository；未拿它當單工具成功方案。 |
| 提示詞限制只用某工具 | 能指示模型行為，但不會撤銷其他工具或縮小 OAuth 權限。 |
| 自架單工具 MCP | 曾準備 Node/OAuth/PAT 專案與模擬測試；沒有完成公開部署或一般 Chat 真實建庫驗收。需要維護執行環境與個人憑證。 |
| GitHub Actions 控制 Repo | 曾準備每人自存 Secret、由請求檔觸發的 worker，7 項模擬測試通過；未部署／未實際建庫。內建 GITHUB_TOKEN 限目前 Repo，不能取代帳號層級建庫授權。它是非同步替代架構，不是已完成的直接 MCP。 |
| GitHub Marketplace | 未上架，也不需要為個人 OAuth 測試上架；上架不會自動完成 ChatGPT 連線。 |

## 來源與判讀

- [GitHub 遠端 MCP、headers 與 URL](https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md)
- [GitHub MCP 工具配置](https://github.com/github/github-mcp-server/blob/main/docs/server-configuration.md)
- [OpenAI 官方 plugin 的 App 宣告](https://github.com/openai/plugins/blob/main/plugins/github/.app.json)
- [OpenAI 官方 plugin 的 MCP 宣告](https://github.com/openai/plugins/blob/main/plugins/github/.mcp.json)
- [ChatGPT 工作區 Action control](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)
- [GITHUB_TOKEN 範圍](https://docs.github.com/en/actions/concepts/security/github_token)

不要複製官方套件中的 OAuth client secret 或其他認證設定；公開設定不代表你的自訂連線可以共用那個 client。不要為了讓篩選生效而把個人權杖交給第三方代理。
