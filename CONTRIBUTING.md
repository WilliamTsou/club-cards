# 參與方式

我們用開源專案最常見的 **Fork 流程**：你沒辦法直接改這個 repo，要先複製一份到自己帳號（fork），改好之後再用 PR（Pull Request）請維護者合併。

第一次參加？先照 [學生講義](docs/handout.md) 從頭做一次，這份是做完之後的查詢用版本。

## 需要的工具

- **Git**：Windows 到 https://git-scm.com/downloads/win 下載安裝，會一起裝好 **Git Bash**；Mac 在終端機打 `git --version` 照提示安裝
- **VS Code**：https://code.visualstudio.com ，延伸模組裝 **Live Server**
- **Windows 一律用 Git Bash 打指令**：`bash xxx.sh`、`cp -r`、`curl -O` 這些指令 PowerShell 不認得。VS Code 按 `Ctrl+Shift+P` → `Terminal: Select Default Profile` → 選 **Git Bash**

## 名詞

| 名詞 | 意思 |
| --- | --- |
| upstream | 這個原始 repo（williamtsou/club-cards），只能拿、不能推 |
| fork / origin | 你複製到自己帳號的那一份，可以推 |
| PR | 請 upstream 把你的修改合併進去的申請單 |
| `~` | 家目錄：Windows 是 `C:\Users\你的名字`，Mac 是 `/Users/你的名字` |
| `/`（開頭） | 根目錄：整台電腦最上層（Git Bash 裡 `/c` 是 C 槽） |

## 第一次：Fork、Token、clone（只做一次）

1. 到 https://github.com/williamtsou/club-cards 右上角按 **Fork** → Create fork
2. 建立 Token（push 用的通行證）：
   - 點 GitHub **右上角你的頭像** → **Settings**。⚠️ 是**你帳號的** Settings，不是 repo 頁面上的 Settings 分頁（也可以直接開 https://github.com/settings/personal-access-tokens ）
   - 左邊最下面 **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**
   - Repository access：**Only select repositories** → 選你的 `club-cards` fork
   - Repository permissions → **Contents：Read and write**
   - 按 Generate token 後**馬上複製**，離開頁面就看不到了
     - Windows：之後按 `Win + V`（剪貼簿歷程記錄）就能找回來。要先按一次 `Win + V` 開啟這個功能，開啟**之後**複製的東西才會被記住
     - Mac：先貼到備忘錄，push 完再刪掉
   - 第一次 push 時，Windows 在登入視窗選 **Token** 分頁貼上；Mac 在 Password 欄貼上（畫面不會顯示字，貼完直接按 Enter）
3. clone **你自己的** fork：
   ```bash
   cd ~
   git clone https://github.com/你的帳號/club-cards.git
   cd club-cards
   ```
4. 把原始 repo 加成 `upstream`，之後才拿得到別人的更新：
   ```bash
   git remote add upstream https://github.com/williamtsou/club-cards.git
   git remote -v
   ```
   看到 `origin`（你的）和 `upstream`（原始的）各兩行就對了

## 每次做事的流程

1. **開 Issue 或認領**：到原始 repo 的 Issues，選一個模板開新的；要做現有的 Issue，就在底下留言「我來做」。已經有人認領的換一個
2. **同步最新版，開分支**：
   ```bash
   git switch main
   git pull upstream main
   git push origin main
   git switch -c <分支名>
   ```
3. **小步 commit**，做完 push 到**你自己的** fork：
   ```bash
   git add .
   git commit -m "feat: 新增 William 的名片"
   git push -u origin <分支名>
   ```
4. **開 PR**：回到 GitHub 按 **Compare & pull request**
   - 方向：左邊 **base** 是 `williamtsou/club-cards` 的 `main`，右邊 **compare** 是 `你的帳號/club-cards` 的 `<分支名>`
   - 意思是「把右邊（你的分支）合併進左邊（老師的 main）」
   - 填好 PR 模板，寫 `Closes #<Issue 編號>`，合併後 Issue 會自動關閉
5. **審核**：收到修改意見時，在**同一個分支**上改完再 `git push`，PR 會自動更新，不用重開；改完在 PR 回覆留言。維護者審核通過後，用 **Squash and merge** 合併
6. **合併後：同步並刪掉用完的分支**
   ```bash
   git switch main
   git pull upstream main
   git push origin main                 # 讓你的 fork 也跟上
   git branch -D <分支名>               # Squash 合併後 Git 認不出已合併，要用大寫 -D
   git push origin --delete <分支名>    # 刪 fork 上的分支（PR 頁按過 Delete branch 就不用）
   git fetch origin --prune             # 清掉已經不存在的遠端分支紀錄
   ```
   只刪**已經 Merged** 的分支。下一個任務開新分支，不要沿用舊的

## 新增自己的名片

1. 建資料夾 `cards/<你的GitHub帳號小寫>/`，放進 `index.html` 和頭像
   - 還沒有名片？複製 `cards/_template/` 來改：`cp -r cards/_template cards/你的帳號`
2. 打開根目錄的 `index.html`，**依 GitHub 帳號的字母順序**，把下面這行插在對的位置：
   ```html
   <li><a href="cards/你的帳號/index.html">你的名字</a></li>
   ```
   例如清單裡已經有 `amy`、`kim`，你是 `cat`，就插在它們中間
3. 用 Live Server 打開確認點得進去

> 為什麼要照字母排？因為只有「改到同一個地方」才會衝突。

## 規則

- 圖片 500KB 以下，檔名只用英文小寫、數字和 `-`
- 路徑用相對路徑（`avatar.jpg`），不要寫自己電腦上的路徑
- 不要改別人的資料夾
- 不要對整份檔案做格式化（會讓所有人都衝突）
- 一個任務 = 一個 Issue = 一個分支 = 一個 PR

## 分支命名

| 用途 | 格式 | 例子 |
| --- | --- | --- |
| 加名片 | `add-<帳號>` | `add-william` |
| 新功能 | `feat/<Issue編號>-<描述>` | `feat/12-search-bar` |
| 修錯 | `fix/<Issue編號>-<描述>` | `fix/15-broken-link` |

## Commit 訊息

開頭寫類型，後面寫「做了什麼」：

- `feat: 新增 William 的名片`
- `fix: 修正目錄連結`
- `docs: 更新 README`
- `style: 調整目錄頁間距`

## merge 是誰合併進誰

**`git merge X` 或 `git pull 遠端 X` = 把 X 合併進「你現在所在的分支」。** 合併前先 `git branch` 確認自己站在哪。

| 動作 | 誰 → 合併進 → 誰 |
| --- | --- |
| 站在 `add-amy`，打 `git pull upstream main` | 老師的 main → **add-amy** |
| 站在 `main`，打 `git pull upstream main` | 老師的 main → **你電腦的 main** |
| 老師在 GitHub 按 PR 的 Merge | 你 fork 上的分支 → **老師的 main** |

## 遇到衝突

兩個 PR 改到同一個地方時，**兩邊都還沒合併前不會顯示衝突**。等其中一個先被合併，另一個 PR 才會出現 **This branch has conflicts**。

**後合併的人負責解。** 先合併的那個人不用動。

1. 在你的分支上，把老師最新的 main 合併進來：
   ```bash
   git switch <你的分支>
   git pull upstream main
   ```
2. 判斷是哪一種情況：
   - 兩邊都是新增 → 兩個都留
   - 別人改了規則（例如改名）→ 照最新版改你的
   - 同一行改成不同意思 → **先別解**，在**你的 PR** 留言 @ 對方和維護者，討論決定後再解
3. 解衝突的四步：
   1. **決定內容**：打開檔案，決定最後要留什麼（VS Code 的 Accept Current / Incoming / Both 按鈕可以幫忙）
   2. **刪掉標記**：`<<<<<<< HEAD`、`=======`、`>>>>>>> 分支名` 三行刪乾淨
   3. `git add .`：告訴 Git 這個檔案解好了
   4. `git commit`：完成這次合併
4. `git push`，PR 會自動更新，衝突提示消失。在 PR 留言「衝突已解決，請再看一次」

解到一半亂掉了：`git merge --abort`，回到 pull 之前。想先練習？執行 `bash scripts/conflict-lab.sh`。

## 溝通方式

### Issue 還是 PR？

| | Issue | PR |
| --- | --- | --- |
| 什麼時候 | 還沒動手 | 已經寫好程式碼 |
| 討論什麼 | 要不要做、做什麼、誰來做 | 這段程式碼對不對、要不要改 |
| 例子 | 認領、問需求、回報進度 | 審核、要求修改、討論衝突怎麼解 |

討論都留在 GitHub 上，不要只用私訊，之後的人才看得到為什麼這樣做。

### 可以這樣說

**在 Issue**

- 認領：「我來做，預計週五前開 PR。」
- 問需求：「搜尋要能搜暱稱嗎？還是只搜名字？」
- 卡住：「我卡在 Dark Mode 重新整理後會跑掉，試過 localStorage 但沒用，有人知道嗎？」

**開 PR 的人（寫在 PR 描述）**

- 做了什麼、怎麼驗證、有改畫面就附截圖（PR 模板會提醒你）
- 改完審核意見：「已依建議修改，請再看一次。」

**審核的人**

- 在 **Files changed** 點那一行旁邊的 **+** 留言，對著程式碼說話
- 說具體的地方和原因：「第 12 行的圖片沒有 alt，螢幕閱讀器會讀不到，建議加上。」
- 用建議或問句，不要用命令：「這裡改用 class 會不會比較好重複使用？」
- 好的地方也可以說：「配色很清楚 👍」
- 沒問題就按 **Review changes → Approve**，可以留一句「LGTM」（Looks Good To Me）

> 審核的是**程式碼**，不是人。不確定的地方用問的，大家都是來練習的。
