# 4. 與遠端倉庫互動 🌐 (Working with Remotes)

到目前為止，我們所有的 Git 操作都發生在您自己的電腦上 (本地倉庫, Local Repository)。現在，我們要學習如何將您的程式碼推送到一個在網路上的中央伺服器 (遠端倉庫, Remote Repository)，以及如何從遠端倉庫拉取最新的變更。

最常見的遠端倉庫託管服務就是 [GitHub](https://github.com)，其他還有 GitLab、Bitbucket 等。

## 為什麼需要遠端倉庫？

*   **🤝 團隊協作**：遠端倉庫是團隊成員共享程式碼的中央樞紐。
*   **☁️ 程式碼備份**：您的電腦可能會壞掉，但只要程式碼在遠端倉庫上，它就是安全的。
*   **🤖 自動化 (CI/CD)**：許多自動化流程 (如自動測試、自動部署) 都需要一個中央倉庫來觸發。

## 核心遠端指令

*   `git clone <倉庫網址>`
    *   **用途**：將一個遠端倉庫完整地複製到您的本地電腦上。它會自動建立與遠端倉庫的連結，預設的遠端名稱為 `origin`。

*   `git remote -v`
    *   **用途**：列出目前專案設定的所有遠端倉庫連結。

*   `git remote add <遠端名稱> <倉庫網址>`
    *   **用途**：為您的本地倉庫新增一個遠端倉庫的連結。最常見的遠端名稱是 `origin`。

*   `git push <遠端名稱> <本地分支名稱>`
    *   **用途**：將您本地分支的提交，上傳 (推送) 到遠端倉庫對應的分支。
    *   **Pro-Tip**: 第一次推送時，常會使用 `git push -u origin main`，`-u` 參數會設定本地 `main` 分支去追蹤遠端 `origin/main` 分支 (稱為設定 **upstream**)。設定好之後，未來您只需要輸入 `git push` 和 `git pull` 即可。

*   `git pull <遠端名稱> <遠端分支名稱>`
    *   **用途**：從遠端倉庫下載最新的變更，並與您本地的分支進行 **合併 (merge)**。
    *   它相當於 `git fetch` + `git merge` 兩個指令的組合。

*   `git fetch <遠端名稱>`
    *   **用途**：從遠端倉庫下載最新的歷史紀錄，但 **不會** 自動合併。它只會更新您本地的「遠端分支指標」(例如 `origin/main`)。
    *   **`pull` vs `fetch`**: `fetch` 更安全，因為它讓您有機會在合併前，先檢視一下遠端的變更 (`git log main..origin/main`)。對於新手，`pull` 更直接；但對於複雜的專案，推薦使用 `fetch` + 手動 `merge` 或 `rebase`。

---

## 💪 動手做：推送到 GitHub

在這個練習中，我們會將本地的 `practice-repo` 推送到您在 GitHub 上建立的一個新倉庫。

1.  **在 GitHub 建立新倉庫** (此步驟需您手動完成)：
    *   登入您的 [GitHub](https://github.com) 帳號。
    *   點擊右上角的 `+` 號，選擇 `New repository`。
    *   **Repository name** 填寫 `git-practice`。
    *   **不要** 勾選任何初始化檔案的選項，我們要從一個完全空白的倉庫開始。
    *   點擊 `Create repository`。

2.  **複製倉庫網址**：
    建立成功後，複製頁面上的 HTTPS 網址，它看起來像 `https://github.com/您的使用者名稱/git-practice.git`。

3.  **連結遠端倉庫**：
    回到您本地的 `practice-repo` 終端機，執行以下指令，將 `<倉庫網址>` 換成您剛剛複製的網址。
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
    git remote add origin <倉庫網址>
    ```
    執行 `git remote -v` 來確認是否連結成功。

4.  **推送您的第一個提交**：
    現在，將您本地的 `main` (或 `master`) 分支推送到 GitHub 上。
    ```bash
    # -u 參數會設定 upstream，讓未來操作更簡單
    git push -u origin main
    ```
    (第一次推送可能需要您登入 GitHub)
    推送成功後，重新整理您在瀏覽器中的 GitHub 倉庫頁面，您會看到所有檔案都已經上傳上去了！🚀

5.  **模擬從遠端拉取變更**：
    *   直接在 GitHub 網站上，點擊 `readme.md` 檔案，然後點擊編輯按鈕 (鉛筆圖示)。
    *   隨意新增一行文字，然後在頁面下方提交變更 (Commit changes)。
    *   現在，遠端倉庫比您本地的要新了。回到您的終端機，執行 `git pull` 來同步這個變更。
    ```bash
    git pull
    ```
    打開您本地的 `readme.md` 檔案，您會發現剛剛在網站上做的修改已經同步下來了。

---

## 下一步

恭喜！您已經完成了 Git 的核心工作流程教學。接下來，在 `05-Advanced-Log-and-Undoing.md` 中，我們將學習如何更優雅地檢視歷史紀錄，以及如何安全地「反悔」。
