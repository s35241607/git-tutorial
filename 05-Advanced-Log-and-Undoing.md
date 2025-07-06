# 5. 進階日誌與撤銷操作 🧐 (Advanced Log & Undoing)

學會了基本操作後，我們來學習如何更優雅地檢視歷史紀錄，以及如何在犯錯時安全地「回到過去」。這部分將介紹 `git log` 的客製化選項，以及 `git reset` 和 `git revert` 這兩個強大的撤銷指令。

## 更美觀的 `git log`

預設的 `git log` 資訊很完整，但有時顯得過於冗長。您可以用一些參數讓輸出更簡潔、更具可讀性。

*   `git log --oneline`: 將每一次的提交壓縮成一行顯示。
*   `git log --graph`: 以 ASCII 圖形的方式，畫出分支與合併的歷史線圖。
*   `git log --decorate`: 顯示每個提交所在的分支名稱或標籤 (tag)。
*   `git log --all`: 顯示所有分支的歷史紀錄。
*   `git log -p`: 顯示每次提交的詳細變更內容 (patch)。
*   `git log --stat`: 顯示每次提交的檔案變更統計。

**✨ 組合使用，效果更佳！**

一個非常實用的組合是：
```bash
git log --graph --oneline --decorate --all
```
> **Pro-Tip**: 您可以為這個超長指令設定一個 Git 別名 (alias)，例如 `git config --global alias.tree "log --graph --oneline --decorate --all"`，之後只需要輸入 `git tree` 即可！

---

## 撤銷變更：`reset` vs `revert`

當您想要撤銷某次提交時，Git 提供了兩種主要方式，它們的哲學完全不同。

### `git reset`：移動歷史，修改過去 (⚠️ 本地專用)

`git reset` 會將您目前所在的分支，強制移動到過去的某個提交點。它會 **改寫歷史**。

它有三種主要模式：

*   `--soft`: 最溫和。只移動分支指標，您之前提交的 **變更內容會被保留在暫存區**。
*   `--mixed` (預設): 移動指標，並將您提交的 **變更內容放回工作目錄**。
*   `--hard`: **🚨 極度危險！** 移動指標，並且 **徹底拋棄** 您在該次提交之後的所有變更。這些內容通常無法找回。

**使用時機**：只在您自己的 **私有分支** 上使用 `reset`，用來整理還沒分享給別人的提交紀錄。**絕對不要對已經推送到遠端共享的公開分支使用 `git reset`**。

### `git revert`：產生新的提交來「反轉」過去 (✅ 公開共享適用)

`git revert` 不會改寫歷史。相反地，它會 **建立一個全新的提交**，這個新提交的內容剛好與您想撤銷的提交完全相反。

**使用時機**：這是 **唯一推薦** 用於撤銷 **公開、共享分支** 上變更的方式。它不會破壞專案歷史，所有的修改都有跡可循，對團隊合作非常安全。

---

## 指定提交：^ 與 ~ 的區別 (Specifying Commits)

當我們使用 `reset`, `revert`, `show` 等指令時，除了用 commit hash，我們還可以用相對引用 (relative reference) 的方式來指定提交，這就是 `^` 和 `~` 的用途。它們讓您能以 `HEAD` (目前所在提交) 為基準，快速地回溯歷史。

### `~` (Tilde): 回溯祖先鏈
`~` 符號主要用於在提交歷史中 **線性地向後移動**。它總是跟隨 **第一個父提交** (first parent)。

*   `HEAD~` 或 `HEAD~1`: 代表 `HEAD` 的第一個父提交 (也就是前一個提交)。
*   `HEAD~2`: 代表 `HEAD` 的第一個父提交的 **第一個父提交** (也就是 `HEAD` 的祖父提交)。

### `^` (Caret): 選擇父提交
`^` 符號的用途更為廣泛，它允許您在一個提交有多個父提交時 (也就是一個 **合併提交**)，明確地選擇要回溯到哪一個父提交。

*   `HEAD^` 或 `HEAD^1`: 代表 `HEAD` 的 **第一個父提交**。在這一點上，它和 `HEAD~1` 是完全一樣的。
*   `HEAD^2`: 代表 `HEAD` 的 **第二個父提交**。這只有在 `HEAD` 是一個合併提交時才有意義。

---

## 💪 動手做：安全地反悔

1.  **檢視漂亮的日誌**：
    進入 `practice-repo`，執行我們學到的組合指令，看看目前的歷史圖。
    ```bash
    cd C:\Users\User\Desktop\新增資料夾\git-tutorial\practice-repo
    git log --graph --oneline --decorate --all
    ```

2.  **做一個錯誤的提交**：
    在 `readme.md` 檔案中，新增一行 `This is a temporary line.`，然後提交它。
    ```bash
    # 手動修改 readme.md
    git add readme.md
    git commit -m "feat: Add a temporary line"
    ```

3.  **用 `revert` 撤銷**：
    我們發現這個提交是多餘的，現在要安全地撤銷它。
    *   執行 `git revert HEAD`。`HEAD` 指向的就是您剛剛的提交。
    *   Git 會跳出一個編輯器讓您確認新提交的訊息 (預設是 `Revert "feat: Add a temporary line"`)，直接儲存並關閉即可。
    *   現在再執行 `git log --oneline`，您會看到多了一個 `Revert` 提交，而 `readme.md` 的內容也恢復原狀了。

4.  **(可選練習) 體驗 `reset`**：
    *   再次新增一個臨時提交：`git commit -am "temp: Another temp commit"`。
    *   執行 `git reset --hard HEAD~1`。這會拋棄最新的提交，讓 `HEAD` 指回前一個提交。
    *   您會發現 `Another temp commit` 這筆紀錄消失了，檔案也恢復了。請務必小心使用此指令！

### 終極後悔藥：`git reflog` 💊

我們提到了 `reset --hard` 和 `ORIG_HEAD`，但如果連 `ORIG_HEAD` 都無法救回您想要的狀態怎麼辦？(例如連續執行了多次危險操作)。

這時，`git reflog` 就是您的終極安全網。Reflog (Reference Log) 記錄了 `HEAD` 在您本地倉庫的 **每一次移動**。無論是 `commit`, `reset`, `merge`, `rebase` 還是 `cherry-pick`，每一次 `HEAD` 的變動都會被記錄下來。

*   **`git reflog`**: 執行此指令，您會看到一個列表，包含了 `HEAD` 的移動歷史，以及每次移動的 hash 值和描述。
*   **救援方式**: 即使您用 `reset --hard` 拋棄了一個提交，它依然存在於 reflog 中。您只需要找到那個您想回去的狀態的 hash (例如 `HEAD@{5}` 對應的 hash)，然後執行 `git reset --hard <hash_from_reflog>`，就可以瞬間將您的專案恢復到那個時間點！

`reflog` 是純本地的，不會被推送到遠端，是屬於您個人的、最強大的救援工具。

---

## 下一步

掌握了如何檢視和修改歷史後，我們可以學習如何使用 `tag` 來標記重要的版本里程碑 (例如 `v1.0`)，以及如何使用 `stash` 來暫存您的工作進度。我們將在 `06-Tagging-and-Stashing.md` 中探討這些主題。
