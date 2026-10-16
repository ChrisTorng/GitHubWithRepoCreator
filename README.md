# GitHubWithRepoCreator

<img src="assets/repo-plus.png" width="96" height="96" alt="Repository with plus sign">

在 **ChatGPT 一般 Chat** 連接 GitHub 官方託管 MCP，使用自己的 GitHub OAuth App，取得包含 `create_repository` 的 GitHub 工具。無須自己架設 MCP Server、代管其他使用者的 PAT、購買 API 額度或透過 Work 建庫。

這個 Repo 提供設定教學、工具清單、驗證提示詞與原創圖示，**不是新的 GitHub Server 實作，也不是裝了 ZIP 就會自動完成 OAuth 的成品**。每個人依下列步驟建立自己的連線即可；不必先 fork 本 Repo。

## 功能與驗證狀態

本次一般 Chat 工作階段曾提供 48 個工具，包含建庫、fork、搜尋、分支、檔案與 commit、PR 與 review、issue、release/tag，以及使用者與團隊查詢。完整工具名稱見 [工具清單](docs/tools.md)。工具數量與供應內容可能因 GitHub 更新、權杖、方案與平台而變動。

| 已驗證項目 | 結果 |
|---|---|
| 個人 OAuth 連線 | 成功，識別正確 GitHub 帳號 |
| `get_me` | 成功 |
| 私人 Repo 中繼資料搜尋 | 成功；不代表讀取過所有檔案內容 |
| `create_repository` | 一般 Chat 成功建立私人 `ChrisTorng/GHRepoCreatorforChatGPT`，初始化 README |
| 其他工具 | 工具定義曾出現，未逐一執行驗證 |
| 只開放 `create_repository` | **未達成；目前採用完整工具版本** |
| 免設定安裝、公開目錄上架 | 未驗證／未進行 |

紀錄日期：2026-10-16，Asia/Taipei。驗證依據為使用者提供的完整測試對話，以及建立後實際查詢到的 Repo ID `1421498706` 與私人可見性；不公開私人 Repo 清單、對話附件或任何憑證。

### 可以取代官方 GitHub 外掛嗎？

對已驗證的查詢與建庫工作，它是可用的替代連線，也補上當次 OpenAI GitHub connector 沒有提供的建庫工具。**不能宣稱完全取代官方整合**：工具 schema、搜尋／索引、檔案處理、帳號與組織政策、產品介面及可用環境不一定相同；48 個工具也不代表 GitHub API 的所有功能。請依自己的實際工作驗收。

OpenAI plugin 套件、OpenAI GitHub connector、GitHub 官方 MCP 是不同層次。此方案連到 `api.githubcopilot.com` 的 GitHub MCP，不是在修改 OpenAI connector。

## 自行建立：成功使用的路徑

### 1. 在 ChatGPT 開啟自訂 MCP 表單

請複製完整網址到已登入 ChatGPT 的瀏覽器：

```text
https://chatgpt.com/plugins
```

選 **＋ → Add custom MCP server**。入口是否可用取決於你的帳號、方案與工作區政策；本案例成功不保證所有帳號均開放。若介面要求 Developer mode，依你的介面啟用。

| 欄位 | 值 |
|---|---|
| Name | `GH with Repo Creator` |
| Description | `Work with GitHub repositories, files, pull requests and issues, including creating new repositories.` |
| Icon | 本 Repo 的 `assets/repo-plus.png`，256 × 256 PNG、10 KB 以下 |
| Server URL | `https://api.githubcopilot.com/mcp/` |
| Authentication | `OAuth` |

展開 Advanced OAuth settings，先記錄表單顯示的 **Callback URL**。本次為：

```text
https://chatgpt.com/connector_platform_oauth_redirect
```

**以自己的介面顯示值為準**；不要直接假設所有新連線都使用這個值。可先保留此表單，另開分頁註冊 OAuth App。

### 2. 在自己的 GitHub 註冊 OAuth App

```text
https://github.com/settings/applications/new
```

| 欄位 | 範例 |
|---|---|
| Application name | `GH with Repo Creator for ChatGPT` |
| Homepage URL | 你的 GitHub 個人頁或自己專案的實際網址 |
| Description | `Connect ChatGPT to GitHub hosted MCP, including repository creation.` |
| Redirect URI | 第一步實際顯示的 Callback URL |

本次 UI 不接受以 `GitHub` 或 `Gist` 開頭的 App 名稱，因此範例使用 `GH`。關閉 callback wildcard matching，使用精確 URI。Device Flow 不屬於本次瀏覽器授權流程，不需為此啟用。

完成註冊後，記下 Client ID，再按 **Generate a new client secret**；本次需另按此鈕，並非註冊後自動有 secret。完成 GitHub 要求的帳號驗證。

Client ID 是應用程式識別碼，Client Secret 是應用程式憑證，兩者都不是 PAT。**不要把 Client Secret、OAuth code、權杖貼到 Chat、截圖、Repo 或設定範例檔。** 直接從 GitHub 填入 ChatGPT 的連線設定。

### 3. 回到 ChatGPT 填 OAuth 設定

| 欄位 | 設定 |
|---|---|
| Registration method | `User-Defined OAuth Client` |
| OAuth client ID | 你剛註冊 App 的 Client ID |
| OAuth client secret | 直接填入剛產生的 Secret |
| Token endpoint auth method | `client_secret_post`，與 GitHub OAuth App 的 secret 交換流程配合 |
| Default scopes | `repo`；先不額外選其他 scope |
| Base scopes | 留白 |
| Auth URL | `https://github.com/login/oauth/authorize` |
| Token URL | `https://github.com/login/oauth/access_token` |
| Registration URL | 留白；`https://example.com` 若只是提示文字不必處理，若是實際值則清除 |
| Authorization server base | `https://github.com/login/oauth` |
| Resource | `https://api.githubcopilot.com/mcp/` |
| OIDC enabled | 關閉 |

優先保留正確的自動 discovery 值，不要用不明教學覆蓋。若 UI 欄位或選項與此不同，先查明差異，不要猜填。其他工具可能需要不同 scope；列出工具不代表它們已具備執行權限。

本次表單曾提示 OpenAI 有預設 OAuth client，但留白後實際得到 `Couldn't register an OAuth client`。後續改用自有 App 才完成連線。你也可以依實際結果選擇內建 client，但不要把那段提示當成已成功授權。

### 4. 建立、安裝並授權

閱讀風險提示，勾選同意後按 **Create as a plugin**。安裝並連接該外掛，繼續至 GitHub 授權頁，確認 App 名稱、開發者、登入帳號與權限後，由你自行決定是否授權。個人建庫不需要額外要求組織授權。

此方案通常要求 `repo` scope，可存取公開與私人 Repo，**不是單純建立新 Repo 的最小權限**。每個使用者自行註冊 App、自行把 secret 填到 ChatGPT，因此本專案作者不收集或管理其他人的 PAT／權杖。ChatGPT 與 GitHub 仍會處理連線所需的憑證；不能宣稱憑證只留在使用者電腦。

### 5. 在一般 Chat 驗收

開新的一般 Chat，只選取剛建立的外掛。先使用 [唯讀驗證提示詞](examples/prompts.md)，確認工具清單與登入者，再指定你確實要建立的 Repo 名稱與可見性。請求中明確說「不要轉交 Work、不要使用 CLI 或其他外掛替代」。

本次 `Allow low-risk` 設定下建庫被核准機制拒絕；改成 `Allow all` 後成功。這是案例觀察，**不代表建庫一定要 Allow all**，也不保證其他模式的確認行為。先看自己的 permission 選項與呼叫錯誤。Allow all 會放寬整個外掛的動作核准，並非只放行建庫；可改用支援逐次確認的模式自行驗收，該模式本次尚未測試。

成功應回傳新 Repo URL／ID；結果不明先查 GitHub，避免自動重試。安裝、OAuth 登入或工具定義出現，都不等於建庫已成功。

## 為什麼不是「只含建庫」？

本次個人設定頁沒有找到逐項停用工具或自訂 header 的入口。GitHub 官方 MCP 支援 `X-MCP-Tools: create_repository`，但我們未成功讓一般 Chat 的註冊 App 連線使用此 header。完整紀錄見 [未成功與未完成的方向](docs/attempts.md)。

本 Repo 不提供誤稱「一般 Chat 可用」的 Only ZIP，不靠提示詞假裝停用工具。若平台日後提供工具白名單，可重新驗證後更新指南。

## 維護與分享

- 到 GitHub Settings → Applications 檢查、撤銷 App 授權；不再使用時也中斷 ChatGPT 連線。
- Secret 洩漏時在 GitHub 撤銷／換發並更新 ChatGPT。不要只刪本機副本。
- Fork 只複製文件與圖示，不會讓他人取得你的 OAuth 連線；每人需自行設定。
- 不需要 GitHub Marketplace 上架；它也不會自動建立 ChatGPT 連線。
- 原創圖示及本專案文件採 [MIT](LICENSE)。不包含 GitHub／OpenAI logo；第三方產品、服務與商標不受本專案 MIT 授權。

## 來源

- [GitHub 官方 MCP 原始碼與遠端服務](https://github.com/github/github-mcp-server)
- [GitHub MCP 設定與工具篩選](https://github.com/github/github-mcp-server/blob/main/docs/server-configuration.md)
- [註冊 OAuth App](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/creating-an-oauth-app)
- [GitHub OAuth 與 PKCE／權杖交換](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps)
- [OAuth scope 範圍](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps)
- [ChatGPT 連線與驗證](https://developers.openai.com/plugins/deploy/connect-chatgpt)
- [ChatGPT MCP 與工作區 Action control](https://help.openai.com/en/articles/12584461-connecting-custom-mcp-servers-in-chatgpt)
