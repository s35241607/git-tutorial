# 3. 解決合併衝突 🤯 (Resolving Conflicts)

當您嘗試合併的兩個分支，各自修改了 **同一個檔案** 的 **同一行** 內容時，Git 就會不知道該如何是好。它無法判斷哪一個版本才是您真正想要的，於是它會暫停合併，並請求您手動介入解決這個「衝突」。

這聽起來可能有點嚇人，但只要理解了原理，解決衝突其實是一個很直接的過程。

## 衝突是如何發生的？

最常見的情境是：

1.  您從 `main` 分支建立了一個 `feature` 分支來開發新功能。
2.  在您開發的同時，另一位同事也修改了 `main` 分支上的某個檔案，並提交了變更。
3.  您在 `feature` 分支上，也修改了 **同一個檔案** 的 **同一行**。
4.  當您完成開發，嘗試將 `feature` 分支合併回 `main` 時，衝突就發生了。

## Git 如何標示衝突？

當衝突發生時，Git 會在合併失敗後，直接在有衝突的檔案中插入一些特殊的標記，來告訴您衝突的範圍。看起來像這樣：

```
<<<<<<< HEAD
這裡是您目前分支 (例如 main) 的內容
=======
這裡是您要合併過來的分支 (例如 feature) 的內容
>>>>>>> feature
```

*   `<<<<<<< HEAD` 到 `=======` 之間，是您目前所在分支 (`HEAD` 指標指向的分支) 的版本。
*   `=======` 到 `>>>>>>> <分支名稱>` 之間，是您要合併進來的那個分支的版本。

## 解決衝突的步驟 ✅

1.  **識別衝突**：執行 `git status`，Git 會明確告訴您哪些檔案處於 `unmerged paths` (未合併路徑) 狀態。
2.  **打開檔案**：用您的程式碼編輯器打開有衝突的檔案。
3.  **決定最終內容**：手動編輯檔案，**刪除所有** Git 插入的特殊標記 (`<<<<<<<`, `=======`, `>>>>>>>`)，並保留您想要的最終版本。您可以選擇其中一個版本，也可以將兩者結合，或寫一個全新的版本。
4.  **加入暫存區**：當您確認檔案內容無誤後，執行 `git add <檔案名稱>`。這個動作是在告訴 Git：「我已經解決好這個檔案的衝突了」。
5.  **完成合併**：當所有衝突都解決並 `add` 之後，執行 `git commit`。您不需要加 `-m` 訊息，因為 Git 會自動產生一個預設的合併訊息，例如 `Merge branch 'feature'`。

> **Pro-Tip**: 許多程式碼編輯器 (如 VS Code) 或專門的 Git 圖形化工具 (如 Sourcetree, GitKraken) 都提供了非常好用的視覺化介面來解決衝突，可以直接點擊按鈕選擇要保留的版本，比手動編輯更方便、更不容易出錯。

---

## 💪 動手做：模擬並解決衝突

讓我們來實際製造並解決一個衝突。

1.  **切換到主分支**：
    確保您在 `main` (或 `master`) 分支上。
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
    git switch main
    ```

2.  **建立新分支並修改檔案**：
    建立一個 `edit-hello` 分支，並在 `hello.txt` 的第一行後面，新增一行 `How are you?`。
    ```bash
    git switch -c edit-hello
    # 現在手動修改 hello.txt，使其內容變為：
    # Hello, Git!
    # How are you?
    ```
    提交您的變更：
    ```bash
    git add hello.txt
    git commit -m "feat: Add a greeting to hello.txt"
    ```

3.  **回到主分支，製造衝突**：
    切換回 `main` 分支。現在，我們在 `main` 分支也修改 `hello.txt`，但在同一位置改成不同的內容。在第一行後面，新增一行 `What's up?`。
    ```bash
    git switch main
    # 現在手動修改 hello.txt，使其內容變為：
    # Hello, Git!
    # What's up?
    ```
    提交您的變更：
    ```bash
    git add hello.txt
    git commit -m "feat: Add a different greeting"
    ```

4.  **嘗試合併，觸發衝突**：
    現在，試著將 `edit-hello` 分支合併進來。
    ```bash
    git merge edit-hello
    ```
    您會看到類似 `CONFLICT (content): Merge conflict in hello.txt` 的錯誤訊息。

5.  **解決衝突**：
    *   打開 `hello.txt`，您會看到 Git 的衝突標記。
    *   手動編輯檔案，決定最終版本。例如，我們決定兩個都保留：
        ```
        Hello, Git!
        How are you?
        What's up?
        ```
    *   儲存檔案後，將其加入暫存區：
        ```bash
        git add hello.txt
        ```
    *   完成合併提交：
        ```bash
        git commit
        ```
        (此時會跳出一個編輯器讓您確認提交訊息，可以直接儲存並關閉)

6.  **清理分支**：
    衝突已解決，`edit-hello` 分支的任務也完成了，可以將它刪除。
    ```bash
    git branch -d edit-hello
    ```

---

## 下一步

恭喜！您已經學會如何處理合併衝突了。接下來，在 `04-Remote-Repositories.md` 中，我們將學習如何與遠端倉庫 (例如 GitHub) 進行互動，這也是團隊協作的關鍵。
