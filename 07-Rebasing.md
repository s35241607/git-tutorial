# 7. 變基：整理歷史的藝術 🎨 (Rebasing)

我們已經學會了使用 `git merge` 來整合不同分支的變更。現在，我們來學習另一種強大的整合工具：`git rebase`。`Rebase` (變基) 的主要目的是讓您的提交歷史保持「線性」，看起來更整潔、更清晰。

## `merge` vs `rebase`：兩種哲學

假設您從 `main` 分支拉出一個 `feature` 分支來開發。在這段時間裡，`main` 分支也多了幾個新的提交。

*   **`git merge` (合併)**：像是一位 **誠實的歷史學家** 🧐。它會忠實地記錄下所有發生的事情，包括分支的產生和合併。這會產生一個新的「合併提交」，將兩個分支的歷史交織在一起。歷史是完全真實的，但可能會很雜亂。

    ![Merge Diagram](https://git-scm.com/images/basic-rebase-1.png)

*   **`git rebase` (變基)**：像是一位 **時間旅行的編輯** ✍️。它會回到過去，將您在 `feature` 分支上的所有提交，一個一個地「重新播放」到 `main` 分支的最新提交之後。它 **改寫了提交歷史**，創造了一個完美的線性故事，彷彿您從一開始就是基於最新的 `main` 分支進行開發。歷史是乾淨的，但不是最初的真相。

    ![Rebase Diagram](https://git-scm.com/images/basic-rebase-2.png)

*圖片來源: git-scm.com*

## 如何使用 `rebase`？

`rebase` 的指令可以理解為：「我要把我 **目前所在** 的分支，接到 **另一個** 分支的頂端」。

**常見流程**：

1.  在 `feature` 分支開發。
2.  `main` 分支有了新的進度。
3.  為了讓 `feature` 分支跟上 `main` 的最新狀態，您執行：
    ```bash
    git switch feature
    git fetch origin # 先從遠端抓取最新資訊
    git rebase origin/main # 將 feature 分支的變更，重新應用在 main 的最新版本之上
    ```
4.  當 `feature` 開發完成後，切換回 `main` 分支，做一次「快轉合併」。
    ```bash
    git switch main
    git merge feature
    ```

## Rebase 的黃金法則 📜

> **🚨 絕對不要對已經推送到遠端、且團隊成員都在使用的公開分支 (例如 `main`, `develop`) 進行 `rebase`。**

`Rebase` 只應該用在您自己的、還沒有分享出去的 **本地分支** 上，用來整理您自己的提交。

---

## 💪 動手做：用 `rebase` 整理歷史

1.  **建立場景**：
    *   回到 `main` 分支，並在 `readme.md` 新增一行 `Main branch updated.`，然後 commit。
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
    git switch main
    # 手動修改 readme.md
    git commit -am "docs: Update main branch"
    ```
    *   建立 `feature-rebase` 分支，並在 `hello.txt` 新增一行 `Testing rebase.`，然後 commit。
    ```bash
    git switch -c feature-rebase
    # 手動修改 hello.txt
    git commit -am "feat: Start feature development for rebase"
    ```

2.  **執行 `rebase`**：
    現在，`feature-rebase` 分支的基礎已經落後於 `main` 了。我們來把它接到 `main` 的最新進度上。
    ```bash
    git rebase main
    ```
    如果遇到衝突，解決方式和 `merge` 類似，解決完後用 `git add .`，然後執行 `git rebase --continue`。

3.  **觀察歷史**：
    執行 `git log --graph --oneline --all`，您會看到一條完美的直線！

4.  **完成合併**：
    ```bash
    git switch main
    git merge feature-rebase
    ```
    您會看到 `Fast-forward` 的訊息，合併完成，歷史依然乾淨。✨

---

## 下一步

`rebase` 最強大的地方在於它的「互動模式」。在下一個章節 `08-Interactive-Rebase.md`，我們將學習如何利用互動式變基來編輯、合併、重新排序您的提交，讓您的提交歷史變得無懈可擊。
