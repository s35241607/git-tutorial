# 9. 精準摘取：🍒 (Git Cherry-Pick)

`git cherry-pick` 是一個非常強大的指令，它允許您從一個分支中「摘取」一個或多個特定的提交，然後像複製貼上一樣，將它們應用到您目前所在的分支上。

## 為什麼需要 Cherry-pick？

想像幾個常見的場景：

*   **緊急修復 (Hotfix)**：您在一個長期開發的 `feature` 分支上修復了一個嚴重的 bug。但這個修復也需要立刻應用到已上線的 `main` 或 `release` 分支，而您又不想將整個還未完成的 `feature` 分支合併過去。這時，您就可以精準地 `cherry-pick` 那個修復 bug 的提交。
*   **功能分享**：您在一個分支上實作了一個很棒的工具函式，團隊的另一個分支也急需這個函式，但兩個分支的開發進度差異很大，不適合合併。
*   **避免錯誤的合併**：您不小心在一個錯誤的分支上做了提交，您可以用 `cherry-pick` 將它「搬」到正確的分支上，然後再用 `git reset` 清理掉錯誤分支上的提交。

`cherry-pick` 的核心思想是：**我只想要你的這次變更，而不是你的全部分支歷史**。

## 如何使用？

**指令格式**：`git cherry-pick <commit_hash>`

您可以一次摘取多個提交：
`git cherry-pick <hash1> <hash2> <hash3>`

您也可以摘取一個連續的範圍：
`git cherry-pick <start_hash>^..<end_hash>` (注意 `start_hash` 後面的 `^`，表示從 `start_hash` 的前一個提交開始，直到 `end_hash`，包含 `start_hash` 和 `end_hash` 本身)

### 衝突處理

如果在摘取過程中發生衝突，Git 會暫停下來，處理方式與 `merge` 或 `rebase` 時完全一樣：

1.  手動編輯檔案，解決衝突。
2.  使用 `git add <檔案名稱>` 將解決後的檔案加入暫存區。
3.  執行 `git cherry-pick --continue` 讓 Git 繼續。
4.  如果想放棄這次摘取，可以執行 `git cherry-pick --abort`。

---

## 💪 動手做：摘取一個重要的修復

1.  **建立場景**：
    *   首先，我們建立一個 `release-v1.0` 分支，模擬一個已經發布的穩定版本。
        ```bash
        cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
        git switch main
        git switch -c release-v1.0
        ```
    *   現在，切換回 `main` 分支，模擬正在進行的開發。我們在 `main` 分支上做兩次提交：一個是重要修復，一個是新功能。
        ```bash
        git switch main
        # 第一次提交：重要修復
        # 手動修改 hello.txt，例如修正一個嚴重的錯字
        git commit -am "fix: Correct a critical typo in hello.txt"
        # 記下這次提交的 hash！

        # 第二次提交：新功能
        # 手動修改 readme.md，新增一行 "[In-Progress] New feature development"
        git commit -am "feat: Start developing a new feature"
        ```

2.  **執行 Cherry-pick**：
    現在，`release-v1.0` 分支需要那個重要的錯字修復，但絕對不能包含還在開發中的新功能。這就是 `cherry-pick` 的完美時機。
    ```bash
    # 切換到需要被修復的分支
    git switch release-v1.0

    # 執行 cherry-pick，<hash> 換成你記下的那個 fix 提交的 hash
    git cherry-pick <hash_of_the_fix_commit>
    ```

3.  **檢視成果**：
    *   檢查 `release-v1.0` 分支的 `hello.txt`，您會發現錯字已經被修正了。
    *   檢查 `readme.md`，您會發現新功能的文字並 **沒有** 被帶過來。
    *   執行 `git log --oneline`，您會看到那個 `fix` 提交被複製過來了，但它有一個 **全新的 hash**，因為它是一個在不同基礎上建立的新提交。

---

## 下一步

掌握了 `cherry-pick` 後，您就又多了一個在複雜分支場景下處理問題的利器。接下來，在 `10-Advanced-Inspection.md` 中，我們將學習如何精準地分析提交紀錄。
