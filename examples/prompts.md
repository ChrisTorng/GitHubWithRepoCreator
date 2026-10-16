# 驗證與使用提示詞

先在一般 Chat 用 @ 選取你自己建立的外掛；範例中的外掛顯示名稱可依實際名稱替換。

## 唯讀工具探索

```text
只檢查我選取的 GH with Repo Creator 外掛。
允許唯讀工具探索與中繼資料查詢，列出實際可呼叫的工具名稱與定義，確認是否包含 create_repository。
不要執行建庫或其他寫入，不轉交 Work，不用 CLI 或其他外掛替代。
若沒有載入工具，請明確回報，不能根據外掛名稱猜測。
```

## 確認帳號

```text
只使用此自訂外掛的 get_me，確認連線的 GitHub 帳號。
不要列出私人 Repo，也不要執行寫入。
```

## 建立確實需要的新 Repo

```text
使用此自訂外掛的 create_repository，在我的個人帳號建立 REPLACE_WITH_YOUR_NEW_REPO_NAME。
private=true，autoInit=true，不指定 organization。
不要轉交 Work、不要用 CLI、不要使用其他外掛替代。
完成後提供 API 回傳的 Repo URL 與 ID；若核准遭拒或結果不明，停止並說明，不自動重試。
```

先替換名稱再送出；此步驟會建立真實 Repo。不要把這個檔案中的範例名稱當成建庫授權。
