# 2. 分支與合併 🌿 (Branching and Merging)

分支 (Branch) 是 Git 最強大也最核心的功能之一。它讓您可以在不影響主要產品線 (通常是 `master` 或 `main` 分支) 的情況下，獨立開發新功能、修復錯誤，或進行各種實驗。

## 什麼是分支？

想像一下，您的專案是一條正在向前延伸的鐵軌 (`main` 分支)。當您要開發一個新功能時，您可以從目前的軌道旁邊，開闢一條新的軌道 (一個新的分支)。您可以在這條新軌道上自由地建造，而原來的鐵軌完全不受影響。

當新功能完成後，您可以再蓋一個匝道，將這條新軌道合併 (Merge) 回主軌道，讓新功能正式成為專案的一部分。

<br>

![Branching Diagram](https://git-scm.com/images/branching-illustration.png)
*圖片來源: git-scm.com*

<br>

### 什麼是 `HEAD`？

`HEAD` 是 Git 中一個非常重要的指標，它永遠指向您 **目前所在的分支** 的 **最新提交**。當您切換分支時，`HEAD` 就會跟著移動到新的分支上。您可以把它想像成是「您目前在哪裡」的標示。

## 常用分支指令

*   `git branch`
    *   **用途**：列出所有本地分支。當前所在的分支會以 `*` 號和不同顏色標示。

*   `git branch <分支名稱>`
    *   **用途**：建立一個新的分支。
    *   **注意**：這個指令只會建立分支，並不會自動切換過去。

*   `git switch <分支名稱>` (推薦) 或 `git checkout <分支名稱>`
    *   **用途**：切換到指定的分支。`git switch` 是較新的指令，語意更清晰。

*   `git switch -c <分支名稱>`
    *   **用途**：建立一個新分支，並 **立即切換** 過去。這是 `git branch` 和 `git switch` 兩個指令的組合，非常方便。👍

*   `git merge <要合併過來的分支名稱>`
    *   **用途**：將另一個分支的變更合併到 **您目前所在** 的分支。
    *   **快轉合併 (Fast-forward)**：如果您的目前分支在分岔出去後，沒有任何新的提交，那麼當您合併 `feature` 分支時，Git 只會單純地將分支指標向前移動，這個過程稱為「快轉」。歷史紀錄會是一條直線。
    *   **合併提交 (Merge Commit)**：如果您目前的分支在分岔後 **有** 新的提交，Git 就會建立一個新的「合併提交」，它的祖先會有兩個，分別指向兩個分支的末端。

*   `git branch -d <分支名稱>`
    *   **用途**：刪除一個已經合併過的分支。如果分支還有未合併的變更，Git 會提示錯誤，這是一種安全機制。
    *   若要強制刪除，可以使用 `-D` (大寫)。

---

## 💪 動手做：開發一個新功能

假設我們要在 `practice-repo` 專案中，新增一個 `readme.md` 檔案來說明專案用途。

1.  **確認目前所在分支**：
    進入 `practice-repo` 資料夾，執行 `git branch`，確認您在 `master` 或 `main` 分支上。
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
    git branch
    ```

2.  **建立並切換到新分支**：
    我們建立一個名為 `add-readme` 的新分支來進行開發。
    ```bash
    git switch -c add-readme
    ```
    再次執行 `git branch`，您會看到 `*` 號已經移到 `add-readme` 前面。

3.  **在新分支上開發**：
    建立一個 `readme.md` 檔案，內容寫上 `This is a practice repository.`。

4.  **提交變更**：
    將新檔案加入暫存區並提交。
    ```bash
    git add readme.md
    git commit -m "feat: Add readme.md"
    ```
    > **Pro-Tip**: 提交訊息的 `feat:` 是一種常見的「提交類型」標籤，有助於分類和自動化。其他常見的還有 `fix:`, `docs:`, `style:`, `refactor:` 等。

5.  **切換回主分支**：
    ```bash
    git switch main
    ```
    切換回來後，您會神奇地發現 `readme.md` 檔案不見了！別擔心，它還安全地待在 `add-readme` 分支裡。

6.  **合併分支**：
    將 `add-readme` 分支的成果合併進來。
    ```bash
    git merge add-readme
    ```
    現在，`readme.md` 檔案就出現在 `main` 分支了！因為 `main` 分支在分岔後沒有新提交，所以這是一次「快轉合併」。

7.  **刪除已完成的分支**：
    功能已經合併，這個分支的任務也完成了，可以將它刪除。
    ```bash
    git branch -d add-readme
    ```

---

## 下一步

有時候，當不同分支修改了同一個檔案的同一行時，合併就會發生「衝突」。在下一個章節 `03-Resolving-Conflicts.md`，我們將學習如何解決這種情況。
