# 11. 忽略檔案 🙈 (.gitignore)

在任何一個軟體專案中，都存在一些不應該被提交到 Git 倉庫的檔案或資料夾。`.gitignore` 檔案就是用來告訴 Git：「請忽略這些檔案，不要追蹤它們的任何變更」。

## 為什麼要忽略檔案？

*   **🛡️ 安全性**：避免意外提交密碼、API 金鑰等敏感資訊 (`.env`, `credentials.json`)。
*   **📦 效率**：避免提交龐大的相依性套件 (`node_modules/`, `vendor/`)，這些應由套件管理器處理。
*   **🧹 整潔性**：避免提交編譯後的產物 (`build/`, `dist/`)、日誌檔 (`.log`) 或作業系統產生的檔案 (`.DS_Store`)。

## `.gitignore` 的語法規則

*   `#` 開頭的行是註解。
*   `*` 匹配任意字元 (不含 `/`)。
*   `**` 匹配任意層級的目錄。
*   `/` 結尾表示忽略整個目錄。
*   `/` 開頭表示只從專案根目錄開始匹配。
*   `!` 開頭表示例外，即不要忽略。

## 範例 `.gitignore` (Node.js)

```gitignore
# Dependencies
/node_modules

# Production
/build
/dist

# dotenv environment variables file
.env*

# Logs
npm-debug.log*
yarn-debug.log*

# Editor directories
.vscode/
.idea/
```

> **Pro-Tip**: 您不需要從頭手寫！[gitignore.io](https://www.toptal.com/developers/gitignore) 是一個超棒的工具，可以根據您的作業系統、開發環境和程式語言，自動產生一份非常完善的 `.gitignore` 範本。

## 全域 `.gitignore`

對於某些您 **所有** 專案都想忽略的檔案 (例如作業系統檔案 `.DS_Store` 或編輯器設定)，您可以設定一個全域的忽略檔案，避免在每個專案都重複設定。

```bash
# 告訴 Git 您的全域忽略檔案在哪裡
git config --global core.excludesfile ~/.gitignore_global

# 現在您可以編輯 ~/.gitignore_global 這個檔案了
```

---

## 💪 動手做：忽略敏感檔案

1.  **建立一個敏感檔案**：
    在 `practice-repo` 的根目錄下，建立一個名為 `secrets.txt` 的檔案，裡面寫上 `My_Password=123456`。

2.  **檢查狀態**：
    執行 `git status`，您會看到 `secrets.txt` 出現在 `Untracked files` 清單中。

3.  **建立 `.gitignore` 檔案**：
    在 `practice-repo` 的根目錄下，建立 `.gitignore` 檔案，並在裡面寫入一行 `secrets.txt`。

4.  **再次檢查狀態**：
    再次執行 `git status`。您會發現 `secrets.txt` 消失了！Git 已經按照您的指示忽略了它。

5.  **提交忽略規則**：
    `.gitignore` 檔案本身 **應該要被提交到 Git 倉庫中**，這樣團隊中的每個人才能共享同一份忽略規則。
    ```bash
    git add .gitignore
    git commit -m "chore: Add .gitignore to exclude secrets"
    ```

**⚠️ 如果檔案已經被追蹤了怎麼辦？**

`.gitignore` 只對 **尚未被追蹤** 的檔案有效。如果您不小心把一個敏感檔案提交上去了，您需要先從 Git 的追蹤清單中移除它：

```bash
# 停止追蹤該檔案，但保留本地檔案
git rm --cached <檔案名稱>

# 現在將檔案名稱加入 .gitignore 並提交
```

---

## 總結

恭喜您！您已經完成了這個 Git 教學系列的所有核心與進階主題。接下來最好的學習方式，就是在您自己的專案中不斷地實踐這些技巧。祝您版本控制愉快！🎉
