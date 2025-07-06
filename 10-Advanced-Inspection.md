# 10. 進階歷史偵查與分析 🕵️ (Advanced History Inspection)

在團隊協作中，理解程式碼的演變歷史、快速定位問題的根源，是每個開發者必備的技能。本章將深入探討 Git 提供的強大工具，幫助您像偵探一樣，精準地分析提交紀錄。

## 1. `git log`：歷史的篩選器

我們已經學過 `git log` 的基本用法，但它真正的力量在於其豐富的過濾選項，讓您可以精確地找到想要的提交。

*   **按作者篩選**：
    `git log --author="<作者名稱>"` (例如 `git log --author="John Doe"`)
    `git log --author="<Email>"` (例如 `git log --author="john.doe@example.com"`)

*   **按提交訊息篩選**：
    `git log --grep="<關鍵字>"` (例如 `git log --grep="fix bug"`)
    `--grep` 支援正規表示式。

*   **按時間篩選**：
    `git log --since="<日期>"` (例如 `git log --since="2 weeks ago"`, `"2023-01-01"`)
    `git log --until="<日期>"` (例如 `git log --until="yesterday"`)

*   **按檔案或目錄篩選**：
    `git log -- <檔案路徑>` (例如 `git log -- src/utils.js`)
    `git log -- <目錄路徑>` (例如 `git log -- src/components/`)
    > **注意**：`--` 是必要的，它告訴 Git 後面的參數是路徑，而不是分支或指令選項。

*   **顯示特定提交的變更**：
    `git log -p <commit_hash>`：顯示該提交引入的詳細變更 (patch)。
    `git log -p -- <檔案路徑>`：只顯示特定檔案的變更。

*   **顯示分支之間的差異**：
    `git log <branch1>..<branch2>`：顯示 `branch2` 有但 `branch1` 沒有的提交。
    `git log <branch1>...<branch2>`：顯示兩個分支共同祖先之後，各自獨有的提交。

## 2. `git blame`：誰動了我的程式碼？

當您看到一行奇怪的程式碼，想知道是誰、何時、在哪次提交中引入的，`git blame` 就是您的最佳工具。

*   **基本用法**：`git blame <檔案路徑>`
    它會逐行顯示檔案內容，並在每行前面標示：
    *   引入該行的提交 hash (部分)
    *   作者名稱
    *   提交日期
    *   行號

*   **篩選範圍**：
    `git blame -L <start_line>,<end_line> <檔案路徑>`：只顯示指定行範圍的 blame 資訊。

## 3. `git diff`：變更的顯微鏡

`git diff` 是用來比較不同版本之間變更的利器。我們之前學過比較工作目錄和暫存區，現在來看看更多用法。

*   **比較兩個提交**：
    `git diff <commit1> <commit2>`：顯示兩個提交之間的差異。

*   **比較兩個分支**：
    `git diff <branch1> <branch2>`：顯示兩個分支頂端之間的差異。

*   **比較工作目錄與特定提交**：
    `git diff <commit_hash> <檔案路徑>`：顯示工作目錄中某個檔案與指定提交版本的差異。

*   **只顯示檔案名稱**：
    `git diff --name-only <commit1> <commit2>`：只顯示兩個提交之間變更的檔案名稱。

## 4. `git bisect`：自動化查找 Bug 引入點 (簡要介紹)

當您發現專案中出現了一個 bug，但不知道是哪次提交引入的，`git bisect` 可以幫助您自動化地進行二分查找，快速定位到引入 bug 的那個提交。

*   **啟動**：`git bisect start`
*   **標記已知好的提交**：`git bisect good <good_commit_hash>`
*   **標記已知壞的提交**：`git bisect bad <bad_commit_hash>`
*   Git 會自動跳轉到中間的提交，您需要測試並告訴 Git 這個提交是 `good` 還是 `bad`。
*   `git bisect good` / `git bisect bad`
*   重複這個過程，直到 Git 找到第一個 `bad` 的提交。
*   **結束**：`git bisect reset`：結束二分查找，回到原來的分支。

---

## 💪 動手做：偵查歷史

1.  **回到 `practice-repo`**：
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\multi-dev-sim\dev1-repo
    # 或者您之前操作的 practice-repo
    ```

2.  **使用 `git log` 篩選**：
    *   查找所有包含 "feat" 關鍵字的提交：`git log --grep="feat" --oneline`
    *   查找特定作者的提交 (將 "Your Name" 替換為您在 Git config 中設定的名稱)：`git log --author="Your Name" --oneline`
    *   查找 `README.md` 檔案的所有變更：`git log -- README.md --oneline`

3.  **使用 `git blame`**：
    *   查看 `README.md` 的每一行是誰修改的：`git blame README.md`

4.  **使用 `git diff`**：
    *   比較您最近的兩個提交：`git diff HEAD~1 HEAD`
    *   比較 `main` 分支和 `feature/add-about-page` 分支 (如果還存在的話)：`git diff main feature/add-about-page`

---

## 下一步

恭喜您！您已經掌握了 Git 中最頂級的歷史偵查技巧。接下來，我們將進入整個教學的最後一個環節：終極指令速查表 `11-Gitignore.md`。
