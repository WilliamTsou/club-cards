# 參與方式

我們用開源專案最常見的 **Fork 流程**：你沒辦法直接改這個 repo，要先複製一份到自己帳號（fork），改好之後再用 PR 請維護者合併。

## 名詞

| 名詞 | 意思 |
| --- | --- |
| upstream | 這個原始 repo（williamtsou/club-cards） |
| fork / origin | 你複製到自己帳號的那一份 |
| PR | 請 upstream 把你的修改合併進去的申請單 |

## 第一次：Fork 與 clone（只做一次）

1. 到 https://github.com/williamtsou/club-cards 右上角按 **Fork** → Create fork
2. 建立 Token（push 用）：Settings → Developer settings → Personal access tokens → **Fine-grained tokens** → Generate new token
   - Repository access：**Only select repositories** → 選你的 `club-cards` fork
   - Repository permissions → **Contents：Read and write**
   - 第一次 push 時，Windows 在登入視窗選 Token 分頁貼上；Mac 在 Password 欄貼上
3. clone **你自己的** fork：
   ```bash
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

1. **開 Issue**：到原始 repo 的 Issues → New issue，選一個模板填好；要做現有的 Issue，就在底下留言「我來做」
2. **同步最新版，開分支**：
   ```bash
   git switch main
   git pull upstream main
   git switch -c <分支名>
   ```
3. **小步 commit**，做完 push 到**你自己的** fork：
   ```bash
   git add .
   git commit -m "feat: 新增 William 的名片"
   git push -u origin <分支名>
   ```
4. **開 PR**：回到 GitHub 按 **Compare & pull request**，確認方向是 `你的帳號/club-cards` → `williamtsou/club-cards`，填好 PR 模板，寫 `Closes #<Issue 編號>`
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
> 剛好跟別人插在同一個位置，就會遇到衝突，照下面「遇到衝突」處理就好。

## 規則

- 圖片 500KB 以下，檔名只用英文小寫、數字和 `-`
- 路徑用相對路徑（`avatar.jpg`），不要寫自己電腦上的路徑
- 不要改別人的資料夾
- 不要對整份檔案做格式化（會讓所有人都衝突）
- 一個 PR 只做一件事

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

## 遇到衝突

PR 頁面出現 **This branch has conflicts**，代表別人先合併了、改到同一個地方：

1. 在你的分支上：`git pull upstream main`
2. 打開有衝突的檔案，決定要留哪些內容，刪掉 `<<<<<<<`、`=======`、`>>>>>>>` 三種標記
   - 兩邊都是新增 → 兩個都留
   - 同一行改成不同意思 → 在 PR 留言問對方，不要自己決定
   - 別人改了規則（例如改名）→ 照最新版改你的程式碼
3. `git add <檔案>` → `git commit` → `git push`，PR 會自動更新

解到一半亂掉了：`git merge --abort`，回到 pull 之前。

想先練習？執行 `bash scripts/conflict-lab.sh`。
