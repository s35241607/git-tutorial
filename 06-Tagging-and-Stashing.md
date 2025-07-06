# 6. 版本標籤與暫存工作 🏷️ (Tagging and Stashing)

本章節將介紹兩個在日常開發中極其實用的工具：`git tag` 用於標記重要的發布點，而 `git stash` 則像一個臨時儲物櫃，讓您可以在不提交未完成工作的情況下，快速切換任務。

## 版本標籤 (Tagging)

當您的專案達到一個重要的里程碑時，例如 `v1.0`、`v2.0.1`，您會希望有一個比 commit hash 更具意義、更永久的標記。這就是 `tag` 的用途。

有兩種主要的標籤類型：

1.  **附註標籤 (Annotated Tags)** (✅ 推薦使用)
    *   這是一個完整的 Git 物件，包含標籤建立者的姓名、Email、日期，以及一段標籤訊息。建議用於正式的產品發布。
    *   **建立指令**：`git tag -a <標籤名稱> -m "您的標籤訊息"`

2.  **輕量標籤 (Lightweight Tags)**
    *   它只是一個指向特定提交的指標，像是一個不會移動的分支。適合用於一些臨時的、非正式的標記。
    *   **建立指令**：`git tag <標籤名稱>`

### 其他標籤指令

*   `git tag`: 列出所有本地標籤。
*   `git show <標籤名稱>`: 顯示特定標籤的詳細資訊。
*   `git push origin <標籤名稱>`: 將 **單一** 標籤推送到遠端倉庫。
*   `git push origin --tags`: 將 **所有** 本地標籤一次性推送到遠端倉庫。
*   `git tag -d <標籤名稱>`: 刪除一個本地標籤。
*   `git push origin --delete <標籤名稱>`: 刪除一個 **遠端** 標籤。

---

## 暫存工作 (Stashing)

想像一個情境：您正在 `feature` 分支上開發一個複雜的功能，程式碼改了一半，還不能提交。突然，老闆跑來跟您說 `main` 分支上有一個緊急的 bug 需要立刻修復！😱

這就是 `git stash` 大顯身手的時刻。

### Stash 核心指令

*   `git stash save "一些描述訊息"`: 將變更儲藏起來並附上描述。
*   `git stash list`: 列出所有儲藏紀錄。
*   `git stash pop`: 取出 **最近一次** 儲藏並應用，同時從列表中刪除。
*   `git stash apply`: 取出儲藏並應用，但 **保留** 在列表中。
*   `git stash drop`: 刪除一個儲藏。
*   `git stash show -p`: 預覽一個儲藏的詳細變更內容。

> **Pro-Tip**: `git stash` 預設只會儲藏 **已追蹤** 的檔案。如果您希望連同 **未追蹤** 的新檔案一起儲藏，可以使用 `git stash -u` 或 `git stash --include-untracked`。

---

## 💪 動手做：發布版本與緊急修復

1.  **標記一個版本**：
    假設我們目前的專案狀態很穩定，準備發布 `v1.0`。
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
    git switch main
    git tag -a v1.0 -m "Official release version 1.0"
    git tag # 查看標籤
    ```

2.  **開始開發新功能**：
    建立一個 `new-feature` 分支，並在 `readme.md` 中新增一行 `Working on a new feature...`，但 **不要** 提交。
    ```bash
    git switch -c new-feature
    # 手動修改 readme.md
    ```

3.  **緊急狀況！儲藏工作**：
    現在，假設需要緊急切換回 `main` 分支。我們先把手邊的工作儲藏起來。
    ```bash
    git stash save "WIP: new feature readme"
    ```
    您會發現工作目錄變乾淨了。

4.  **切換分支並修復 Bug** (此處為模擬)：
    ```bash
    git switch main
    # ... 在這裡進行 bug 修復並 commit ...
    ```

5.  **回來繼續工作**：
    Bug 修好了，現在回到 `new-feature` 分支，把之前的工作拿回來。
    ```bash
    git switch new-feature
    git stash pop
    ```
    您會看到 `Working on a new feature...` 這行文字又回來了！您可以繼續您的開發工作。

---

## 下一步

您已經學會了 Git 中許多非常實用的工具。接下來，我們可以探討一個更進階的主題：`git rebase`。它提供了另一種合併分支的方式，可以讓您的提交歷史變得非常線性、整潔。我們將在 `07-Rebasing.md` 中詳細介紹它。
