# 12. Git 終極指令速查表 🚀 (The Ultimate Cheatsheet)

這是一份更完整、更深入的 Git 指令及參數快速參考指南，旨在成為您開發時不可或缺的案頭寶典。

---

## ⚙️ 1. 設定與初始化 (Setup & Init)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git config` | `--global user.name "Your Name"` | 設定全域使用者名稱 |
| | `--global user.email "you@example.com"` | 設定全域使用者 Email |
| | `--global alias.<name> <command>` | 為指令設定全域別名 (如 `alias.tree "log --graph..."`) |
| | `--global core.excludesfile <path>` | 設定全域 .gitignore 檔案 |
| `git init` | | 在目前目錄初始化一個新的 Git 倉庫 |
| `git clone` | `<repo_url>` | 從遠端網址複製一個倉庫到本地 |

---

## 📝 2. 日常工作流程 (Basic Workflow)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git add` | `<file>` / `.` | 將檔案變更加入暫存區 |
| | `-p` | 進入互動模式，逐塊檢視變更並決定是否暫存 |
| | `-i` | 進入互動模式，可選擇檔案或區塊進行操作 |
| `git commit` | `-m "Message"` | 提交暫存區的內容並附上訊息 |
| | `-am "Message"` | 將所有 **已追蹤** 檔案的變更加入暫存區並提交 |
| | `--amend` | 修改 **最後一次** 的提交 (可改訊息或內容) |
| `git status` | | 顯示工作目錄和暫存區的狀態 |
| | `-s` or `--short` | 以簡短的格式顯示狀態 |
| | `-b` or `--branch` | 顯示分支資訊 |
| `git mv` | `<old> <new>` | 移動或重新命名檔案 (等同於 `mv` + `git add` + `git rm`) |
| `git rm` | `<file>` | 從工作目錄和暫存區中刪除檔案 |
| | `--cached <file>` | **僅** 從暫存區中刪除 (停止追蹤)，但保留本地檔案 |

---

## 🌿 3. 分支與合併 (Branching & Merging)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git branch` | | 列出所有本地分支 |
| | `-a` | 列出所有本地和遠端分支 |
| | `-vv` | 顯示本地分支及其追蹤的遠端分支的詳細資訊 |
| | `--merged` / `--no-merged` | 顯示已合併/未合併到當前分支的分支 (常用於清理) |
| | `-d <branch>` | 刪除一個 **已合併** 的分支 |
| | `-D <branch>` | **強制** 刪除一個分支 |
| `git switch` | `<branch>` | 切換到指定分支 |
| | `-c <branch>` | 建立新分支並立即切換過去 |
| `git checkout`| `-` | 切換到 **上一個** 所在的分支 |
| `git merge` | `<branch>` | 將指定分支的變更合併到目前分支 |
| | `--no-ff` | 強制產生一個合併提交 |
| | `--abort` | 取消一次失敗的合併，回到合併前的狀態 |

---

## 🔍 4. 比較與檢查 (Diffing & Inspecting)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git diff` | | 顯示 **工作目錄** 與 **暫存區** 之間的差異 |
| | `--staged` or `--cached` | 顯示 **暫存區** 與 **最新提交 (HEAD)** 之間的差異 |
| | `<commit1> <commit2>` | 顯示兩個提交之間的差異 |
| | `<branch1>..<branch2>` | 顯示兩個分支頂端之間的差異 |
| | `--name-only` | 只顯示變更的檔案名稱 |
| `git log` | `--oneline` | 將每個提交壓縮成一行顯示 |
| | `--graph` | 以圖形化顯示分支歷史 |
| | `--decorate` | 顯示分支和標籤名稱 |
| | `--all` | 顯示所有分支的歷史 |
| | `--author="Name"` | 只顯示特定作者的提交 |
| | `--grep="Keyword"` | 只顯示提交訊息包含關鍵字的提交 |
| | `--since="date"` / `--until="date"` | 只顯示特定時間範圍內的提交 |
| | `-p <file>` | 顯示特定檔案的提交歷史及其詳細變更 |
| `git show` | `<commit_or_tag>` | 顯示某個提交、標籤或樹物件的詳細資訊 |
| `git blame` | `<file>` | 逐行顯示檔案內容，並標示每一行最後是由誰在哪次提交中修改的 |
| | `-L <start>,<end>` | 只顯示指定行範圍的 blame 資訊 |
| `git bisect` | `start` / `good` / `bad` / `reset` | 自動化查找引入 bug 的提交 |

---

## 🌐 5. 遠端與同步 (Remotes & Syncing)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git remote` | `-v` | 列出所有遠端倉庫的詳細資訊 |
| | `set-url <name> <new_url>` | 修改一個遠端的 URL |
| `git fetch` | `<remote>` | 從遠端下載最新的歷史紀錄，但 **不** 自動合併 |
| | `--prune` or `-p` | 在下載前，刪除本地不存在於遠端的追蹤分支 |
| `git pull` | | 從遠端下載並 **自動合併** (`fetch` + `merge`) |
| | `--rebase` | 使用 `rebase` 的方式來取代 `merge` 進行整合 |
| `git push` | `-u origin <branch>` | 推送並設定上游 (upstream) 追蹤關係 |
| | `--force-with-lease` | 更安全的強制推送 (若遠端有新提交則會失敗) |
| | `--tags` | 將所有本地標籤一次性推送到遠端 |

---

## ⏪ 6. 撤銷與救援 (Undoing & Rescuing)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git reset` | `<commit>` | **(危險)** 將分支指標移動到指定提交，改寫歷史 |
| | `--hard` | 移動指標，並 **拋棄** 所有後續變更 |
| `git revert` | `<commit>` | **(安全)** 建立一個新的提交，用來反轉指定提交的變更 |
| `git cherry-pick` | `<commit_hash>` | 將 **另一個分支** 的某個特定提交，複製並應用到目前分支 |
| `git clean` | `-n` or `--dry-run` | **預覽** 將會被刪除的未追蹤檔案 |
| | `-f` | **(危險)** 強制刪除所有未追蹤的檔案 |
| | `-fd` | **(極度危險)** 強制刪除所有未追蹤的檔案和 **目錄** |
| `git reflog`| | 顯示 `HEAD` 的所有移動日誌，終極救援工具 |

---

## 🧰 7. 進階工具 (Advanced Tools)

| 指令 | 常用參數 | 說明 |
| --- | --- | --- |
| `git rebase` | `-i HEAD~N` | 進入互動模式，修改最近的 N 個提交 |
| | `--abort` / `--continue` | 取消 / 繼續 rebase |
| `git stash` | | 將未提交的變更儲藏起來 |
| | `push -m "msg"` | 儲藏並附上描述訊息 |
| | `pop` / `apply` | 取出儲藏並應用 |
| | `-u` or `--include-untracked` | 連同未追蹤的檔案一起儲藏 |
| | `branch <name> <stash_id>` | 從一個儲藏建立一個新分支 |
| | `clear` | 清空所有儲藏 |
| `git tag` | `-a <tag> -m "msg"` | 建立一個附註標籤 |
| | `-f` | 強制移動一個標籤到當前提交 |

---

## 📍 8. 特殊指標 (Special Pointers)

| 指標 | 說明 |
| --- | --- |
| `HEAD` | 永遠指向您 **目前所在的分支** 的 **最新提交**。是您在專案中的「現在位置」。 |
| `ORIG_HEAD` | 這是一個 **備份指標**，用來記錄 **危險操作** (如 `merge`, `reset`, `rebase`) 發生 **之前** `HEAD` 所在的位置。 |
| | **救援範例**：如果您執行 `git reset --hard` 後發現做錯了，可以執行 `git reset --hard ORIG_HEAD` 來快速回到操作前的狀態。 |
| `FETCH_HEAD` | 記錄了上一次 `git fetch` 時，從遠端抓取到的每個分支的頂端提交。`git pull` 內部會用到它。 |
