# 8. 互動式變基：提交的魔法 🪄 (Interactive Rebase)

如果說 `git rebase` 是用來整理分支歷史的工具，那麼「互動式變基」(`git rebase -i`) 就是一個能讓您精雕細琢每一次提交的瑞士軍刀。它讓您在推送程式碼前，有機會將本地的、凌亂的提交歷史，整理成一段清晰、有意義的故事。

## 為什麼需要互動式變基？

在開發過程中，我們的提交紀錄常常是混亂的，例如：

*   `"WIP"` (Work in progress, 開發中)
*   `"Fix typo"` (修正錯字)
*   `"Oops, forgot a file"` (啊，忘了一個檔案)

在將功能合併到主分支前，我們希望能將這些零碎的提交合併成一個或幾個有完整意義的提交，例如 `"feat: Implement user authentication"`。

## 如何啟動？

**指令格式**：`git rebase -i <您想修改的範圍的起點>`

一個常見的用法是 `git rebase -i HEAD~3`，這代表：「我要修改從 `HEAD` (當前提交) 往前的 3 個提交」。

執行後，Git 會打開一個文字編輯器，裡面列出了您指定範圍內的所有提交。

## 核心互動指令

您需要做的就是修改每一行前面的 `pick`，把它換成您想執行的動作。最常用的有：

*   `pick` (p): 保留這個提交，不做任何改變。
*   `reword` (r): 保留這個提交，但 **修改** 它的提交訊息。
*   `edit` (e): 保留這個提交，但 `rebase` 會暫停在該次提交，讓您有機會 **修改檔案內容**。
*   `squash` (s): 將這個提交 **合併到前一個** 提交中，並讓您為合併後的新提交撰寫訊息。
*   `fixup` (f): 與 `squash` 類似，但會 **拋棄** 這個提交的訊息，直接使用前一個提交的訊息。對於修正錯字這類小提交特別好用。
*   `drop`: 直接 **刪除** 該次提交。

---

## 💪 動手做：清理您的提交歷史

1.  **製造一些凌亂的提交**：
    *   建立一個 `feature-interactive` 分支。
    *   **第一次提交**：在 `hello.txt` 中新增 `function login() {}`，然後提交：`git commit -am "feat: Add login function shell"`。
    *   **第二次提交**：發現上個提交有錯字，修改為 `function login() { ... }`，然後提交：`git commit -am "fix: Correct typo in login function"`。
    *   **第三次提交**：為專案新增 `.env` 檔案並加入 `.gitignore`，然後提交：`git commit -am "chore: Add gitignore for env file"`。

2.  **啟動互動式變基**：
    我們來整理剛剛這 3 個提交。
    ```bash
    git rebase -i HEAD~3
    ```

3.  **編輯指令**：
    在打開的編輯器中，我們計畫這麼做：
    *   將修正錯字的提交 (`fix:`) 合併到它修正的那個提交 (`feat:`) 中，並且不需要保留 `fix:` 的訊息。
    *   將 `.gitignore` 的提交移到最前面，因為它和功能本身比較無關。

    將檔案內容修改成這樣 (調整順序並修改 `pick`):
    ```
pick <hash_of_chore> chore: Add gitignore for env file
pick <hash_of_feat> feat: Add login function shell
fixup <hash_of_fix> fix: Correct typo in login function
    ```

4.  **完成 Rebase**：
    *   儲存並關閉檔案。
    *   因為我們用了 `fixup`，Git 不會停下來讓您編輯訊息，它會自動拋棄第二個提交的訊息。

5.  **檢視成果**：
    執行 `git log --oneline`，您會發現原本 3 個凌亂的提交，現在變成了 2 個乾淨、有意義的提交，而且順序也改變了！✨

---

## 下一步

恭喜！您已經掌握了 Git 中最頂級的「歷史整理術」。接下來，在 `09-Cherry-Picking.md` 中，我們將學習如何精準地摘取提交。
