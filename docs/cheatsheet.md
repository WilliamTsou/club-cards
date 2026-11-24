# 速查表

忘記了就回來查，不用背。Mac 把 `Ctrl` 換成 `Cmd`、`Alt` 換成 `Option`（有例外的會另外寫）。

## 系統與通用

| 想做什麼 | Windows | Mac |
| --- | --- | --- |
| 全選 | `Ctrl + A` | `Cmd + A` |
| 複製 / 貼上 / 剪下 | `Ctrl + C` / `V` / `X` | `Cmd + C` / `V` / `X` |
| 復原 / 重做 | `Ctrl + Z` / `Ctrl + Y` | `Cmd + Z` / `Cmd + Shift + Z` |
| 存檔 | `Ctrl + S` | `Cmd + S` |
| 尋找 | `Ctrl + F` | `Cmd + F` |
| 切換視窗 | `Alt + Tab` | `Cmd + Tab` |
| 改檔名（檔案總管 / VS Code 檔案列表） | `F2` | 選取後按 `Enter` |
| 開檔案總管 / Finder | `Win + E` | `Cmd + 空白鍵` 搜尋 Finder |
| 截圖（選範圍） | `Win + Shift + S` | `Cmd + Shift + 4` |
| 顯示桌面 | `Win + D` | `F11` 或觸控板四指張開 |

## VS Code

| 想做什麼 | 快捷鍵 |
| --- | --- |
| 快速開檔案 | `Ctrl + P` |
| 指令面板（什麼都能搜） | `Ctrl + Shift + P` |
| 開 / 關終端機 | ``Ctrl + ` ``（Esc 下面那顆） |
| 整個專案搜尋 / 取代 | `Ctrl + Shift + F` / `Ctrl + Shift + H` |
| 註解 / 取消註解 | `Ctrl + /` |
| 整行上下移動 | `Alt + ↑` / `Alt + ↓` |
| 往下複製一行 | `Shift + Alt + ↓`（Mac：`Shift + Option + ↓`） |
| 選取下一個相同的字（多游標） | `Ctrl + D` |
| 側邊欄開 / 關 | `Ctrl + B` |

> ⚠️ 不要在這個 repo 用「格式化整份文件」（`Shift + Alt + F`），會改到大家的行，所有人都衝突。

## 終端機技巧

| 想做什麼 | 怎麼做 |
| --- | --- |
| 自動補完檔名 / 資料夾名 | 打一半按 `Tab`（按兩下列出所有可能） |
| 叫出上一個指令 | `↑` |
| 中斷正在跑的東西 | `Ctrl + C`（終端機裡**不是**複製） |
| 清畫面 | `clear` 或 `Ctrl + L` |
| 游標跳到行首 / 行尾 | `Ctrl + A` / `Ctrl + E` |
| 複製 / 貼上（Git Bash） | 選取文字就是複製；右鍵或 `Shift + Insert` 貼上 |
| 複製 / 貼上（VS Code 終端機） | `Ctrl + C`（有選取文字時）/ `Ctrl + V` |
| 卡在奇怪的畫面（Vim） | 按 `Esc`，打 `:q!` 再按 `Enter` 離開 |
| 卡在一頁一頁的畫面（`git log` 之類） | 按 `q` 離開 |

## 終端機指令

| 指令 | 做什麼 | 例子 |
| --- | --- | --- |
| `pwd` | 我現在在哪 | `pwd` |
| `ls` | 這裡有什麼 | `ls`、`ls -a`（含隱藏檔）、`ls -la`（詳細）、`ls -R`（整棵樹） |
| `cd` | 走進資料夾 | `cd campus`、`cd ..`（上一層）、`cd ~`（家目錄）、`cd -`（剛剛那裡） |
| `mkdir` | 建資料夾 | `mkdir checkin` |
| `touch` | 建空檔案 | `touch hello.txt` |
| `cat` | 看檔案內容 | `cat clue.txt` |
| `echo` | 印字 / 寫進檔案 | `echo hi > a.txt`（覆蓋）、`echo hi >> a.txt`（加在後面） |
| `cp` | 複製 | `cp a.txt b.txt`、`cp -r 資料夾 新資料夾` |
| `mv` | 移動 / 改名 | `mv a.txt docs/`、`mv old.txt new.txt` |
| `rm` | 刪除（**不會進資源回收筒**） | `rm a.txt`、`rm -r 資料夾` |
| `code .` | 用 VS Code 開這裡 | `code .` |
| `curl -O` | 下載檔案 | `curl -O https://.../file.sh` |
| `bash` | 執行 .sh 腳本 | `bash scripts/conflict-lab.sh` |

> `tree` 在 Git Bash 和 Mac 都沒有內建，用 `ls -R` 代替。

## Git 指令

| 想做什麼 | 指令 |
| --- | --- |
| 看現在狀態（不知道怎麼辦就打這個） | `git status` |
| 放進暫存區 | `git add 檔名`、`git add .`（全部） |
| 存檔 | `git commit -m "feat: 做了什麼"` |
| 看改了什麼 | `git diff` |
| 看紀錄 | `git log --oneline` |
| 複製 repo 到電腦 | `git clone 網址` |
| 看遠端有哪些 | `git remote -v` |
| 開新分支並切過去 | `git switch -c 分支名` |
| 切換分支 / 看所有分支 | `git switch 分支名` / `git branch` |
| 拿原始 repo 的最新版 | `git pull upstream main` |
| 推到自己的 fork | `git push -u origin 分支名`（第一次）、之後 `git push` |
| 解衝突解到一半想放棄 | `git merge --abort` |
| 刪掉已合併的分支（電腦上） | `git branch -D 分支名`（Squash 合併後要用大寫） |
| 刪掉 fork 上的分支 | `git push origin --delete 分支名` |
| 清掉不存在的遠端分支紀錄 | `git fetch origin --prune` |
| 看所有分支（含遠端） | `git branch -a` |
| 丟掉還沒 commit 的修改 | `git restore 檔名` |
| commit 訊息打錯（還沒 push） | `git commit --amend` |

## 常見錯誤

| 看到 | 原因 | 解法 |
| --- | --- | --- |
| `command not found` | 沒裝，或裝完沒重開終端機 | 重開 VS Code / 終端機 |
| `not a git repository` | 不在 repo 資料夾裡 | `pwd` 看在哪，`cd` 到對的地方 |
| `Permission denied` / 403 | push 到別人的 repo，或 Token 權限不對 | 確認是 push 到 `origin`（你的 fork）；Token 要選到你的 fork、Contents 要 Read and write |
| `Authentication failed` | 密碼欄打了 GitHub 密碼，或 Token 過期 | 密碼欄要貼 Token；過期就重新產生 |
| `not fully merged`（刪分支時） | Squash 合併後 Git 認不出已合併 | 確認 PR 已 Merged，改用 `git branch -D` |
| `rejected (fetch first)` | 遠端有你沒有的新東西 | 先 `git pull`，再 push |
| `bash: gh: command not found`（在 PowerShell 打 bash） | 叫到 WSL 的 bash | 改用 Git Bash |
| 網頁上出現 `<<<<<<<` | 解衝突忘了刪標記 | 刪掉標記再 commit |
| 忘記開分支就 commit 在 main | 修改進了 main，跟老師的 main 不一樣 | `git switch -c 新分支` → `git fetch upstream` → `git branch -f --no-track main upstream/main` |
