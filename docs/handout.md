# 專案組第一次聚會：學生講義

> 今天的目標：**把你的名片放上 [名片牆](https://williamtsou.github.io/club-cards/)**。
> 過程中你會學到終端機、Git，還有業界工程師每天在用的開發流程。

**怎麼求救**：上課會發綠色和紅色兩種便利貼。做完一段，把**綠色**貼在螢幕上緣；卡住了就換貼**紅色**，老師或助教看到紅色就會過來（沒拿到便利貼的話，直接舉手也可以）。
指令和快捷鍵不用背，忘了就查 [速查表](cheatsheet.md)。

文中的 `你的帳號` 都要換成**你的 GitHub 帳號**（`github.com/` 後面那串）。
程式碼區塊裡 `#` 後面的文字是**說明**，不用打進去。

---

## 0. 課前準備（在家做好）

### 0-1 註冊 GitHub

- [ ] 到 https://github.com 註冊帳號，**完成 email 驗證**（沒驗證不能 Fork）

### 0-2 安裝 Git（Windows 會一起裝好 Git Bash）

**Windows**

1. 到 https://git-scm.com/downloads/win 下載 **Git for Windows**（64-bit Setup）
2. 安裝時大部分選項按 **Next** 用預設值就好，只有一個建議改：
   - 「Choosing the default editor」那一頁，選 **Use Visual Studio Code as Git's default editor**（還沒裝 VS Code 的話先裝 VS Code 再回來裝 Git）
3. 裝完在開始選單搜尋 **Git Bash**，打開輸入 `git --version`，有出現版本號就成功了

**Mac**

1. 打開「終端機」（Launchpad 搜尋 Terminal）
2. 輸入 `git --version`，如果跳出視窗要你安裝「命令列開發者工具」，按 **安裝**
3. 裝完再輸入一次 `git --version`，有出現版本號就成功了

> **為什麼 Windows 要用 Git Bash？**
> 今天用到的 `bash xxx.sh`、`cp -r`、`curl -O`、`touch` 都是 bash 的指令。
> Windows 內建的 PowerShell 不認得它們（在 PowerShell 打 `bash` 還會叫到別的程式），所以 Windows 一律用 **Git Bash**。
> Mac 的「終端機」本來就能用這些指令，不用另外裝。

### 0-3 安裝 VS Code

1. 到 https://code.visualstudio.com 下載安裝
2. Windows 安裝時，**勾選「加入 PATH」和「將『以 Code 開啟』加入右鍵選單」**（終端機打 `code .` 才會有用）
3. 打開 VS Code，左邊「延伸模組」（四個方塊的圖示）搜尋並安裝 **Live Server**
4. **Windows**：按 `Ctrl+Shift+P` → 輸入 `Terminal: Select Default Profile` → 選 **Git Bash**，之後 VS Code 裡的終端機就會是 Git Bash
5. **Mac**：按 `Cmd+Shift+P` → 輸入 `Shell Command: Install 'code' command in PATH` → 按 Enter，終端機打 `code .` 才會有用

### 0-4 準備名片

- [ ] 準備好 w10 的名片（`index.html` + 頭像，頭像 500KB 以下、檔名用英文）
  - 沒做完也沒關係，今天可以用範本

---

## 1. 環境設定（10 分鐘）

### 1-1 打開終端機

VS Code 按 ``Ctrl + ` ``（Esc 下面那顆），下方會出現終端機。

✅ 檢查：Windows 的提示符號裡要有 `MINGW64`。如果看到的是 `PS C:\...`，代表開成 PowerShell 了，回去做課前準備的最後一步。

### 1-2 告訴 Git 你是誰（只要做一次）

```bash
git config --global user.name "你的名字"           # 你的 commit 上會顯示這個名字
git config --global user.email "你註冊 GitHub 的 email"  # 用同一個 email，GitHub 才認得是你
git config --global pull.rebase false              # pull 時用「合併」的方式整合別人的修改
git config --global core.editor "code --wait"      # 需要打訊息時用 VS Code 開，不會跳進看不懂的 Vim
```

- `git config`：改 Git 的設定
- `--global`：套用到這台電腦的所有專案（不加就只對目前這個資料夾有效）

✅ 檢查：`git config --global --list` 看得到你的名字和 email。

### 先懂四件事

1. **Git** 是存檔工具，在你的電腦裡；**GitHub** 是放專案的網站
2. 存檔分兩步：`git add`（把要存的東西放進信封）→ `git commit`（封口寄出，留下一筆紀錄）
3. 今天會有三個地方：

| 名字 | 是什麼 | 你能改嗎 |
| --- | --- | --- |
| `upstream` | 老師的 repo（williamtsou/club-cards） | ❌ 只能用 PR 申請 |
| `origin` | 你 fork 到自己帳號的那一份 | ✅ |
| 你的電腦 | clone 下來的那一份 | ✅ |

4. **HEAD = 「你現在站的位置」**。它指向你目前所在分支的最新一筆 commit。
   - `git log --oneline` 會看到 `(HEAD -> add-你的帳號)`，意思是「你現在在 add-你的帳號 這個分支上」
   - 切換分支（`git switch`），HEAD 就跟著移過去
   - 解衝突時看到的 `<<<<<<< HEAD`，就是「你現在這邊的版本」

---

## 2. 終端機尋寶（15 分鐘）

終端機就是「用打字操作資料夾」。資料夾是一間一間房間：

| 指令 | 英文 | 意思 |
| --- | --- | --- |
| `pwd` | print working directory | 我現在在哪？ |
| `ls` | list | 這個房間有什麼？ |
| `cd 房間名` | change directory | 走進去；`cd ..` 退回上一層、`cd ~` 回家 |

路徑裡常看到的符號：

| 符號 | 意思 | Windows（Git Bash）實際是 | Mac 實際是 |
| --- | --- | --- | --- |
| `~` | **家目錄**：你自己的使用者資料夾 | `C:\Users\你的名字`（Git Bash 顯示成 `/c/Users/你的名字`） | `/Users/你的名字` |
| `/`（開頭） | **根目錄**：整台電腦最上層 | Git Bash 的最上層，`/c` 就是 C 槽 | 整顆硬碟最上層 |
| `/`（中間） | 分隔資料夾 | `cards/amy/index.html` = cards 裡的 amy 裡的 index.html | 同左 |
| `.` | 目前這個資料夾 | `code .` = 用 VS Code 打開這裡 | 同左 |
| `..` | 上一層資料夾 | `cd ..` = 退回上一層 | 同左 |

> 今天的東西都放在 `~`（家目錄）底下。找不到檔案時，先打 `pwd` 看看自己在哪。

### 2-1 下載尋寶地圖

```bash
cd ~              # 回到家目錄（~ 就是你的使用者資料夾）
curl -O https://williamtsou.github.io/club-cards/scripts/terminal-quest.sh
                  # curl 從網路抓檔案；-O 用網址上的原檔名存下來
bash terminal-quest.sh   # 用 bash 執行這個腳本，它會幫你建好尋寶地圖
```

### 2-2 開始尋寶

```bash
cd ~/terminal-quest
cat README.txt    # cat 把檔案內容印在畫面上
```

接下來**跟著每個線索走**，每一關都會教你一個新指令（`ls -a`、`mkdir`、`mv`、`rm`、`echo`…），線索裡都有說明它在做什麼。

兩個超好用的技巧：

- 名字打一半按 **`Tab`**，會自動補完
- 按 **`↑`**，會叫出上一個指令

### 2-3 過關檢查

走到宿舍、照最後一個線索做完後：

```bash
cd ~/terminal-quest
bash check.sh
```

✅ 看到 🎉 全部過關，貼上綠色便利貼！

> ⚠️ `rm` 刪掉的東西**不會進資源回收筒**，用之前想清楚。
> ⚠️ 終端機裡的 `Ctrl + C` 是「中斷」，不是複製。

---

## 3. 新人報到：完整開發流程（30 分鐘）

> **情境**：你今天是新公司的新人工程師。技術主管（老師）交給你第一個任務：**把自己加進團隊頁**。
> 你要照公司規定的流程來：**開 Issue → Fork → 開分支 → commit → push → 開 PR → 等審核 → 收尾**。

先看老師示範一次，再自己做。每做完一步就打勾。

### Step 1：開 Issue（先說你要做什麼）

> 業界的規矩：**動手之前先開 Issue**，讓大家知道你要做什麼，避免兩個人做同一件事。

- [ ] 打開 https://github.com/williamtsou/club-cards → **Issues** → **New issue**
- [ ] 選 **「新增名片」** 模板，填好送出
- [ ] 記下 Issue 編號（標題後面的 `#數字`），等一下會用到：`#____`

### Step 2：Fork（複製一份到你的帳號）

> 你沒有權限改老師的 repo，所以先複製一份到自己帳號，在自己那份上改。

- [ ] 在 repo 首頁右上角按 **Fork** → **Create fork**
- [ ] 網址變成 `github.com/你的帳號/club-cards` 就對了

### Step 3：建立 Token（push 用的通行證）

> 用 HTTPS 把東西推上 GitHub 時，**GitHub 不接受你的帳號密碼**，要改用 **Token（個人存取權杖）**。
> Token 就像一張「只能開特定門的鑰匙卡」：你決定它能進哪個 repo、能做什麼、什麼時候過期。

1. 點 GitHub **右上角你的頭像** → **Settings**
   - ⚠️ 是**你帳號的** Settings（網址是 `github.com/settings/...`），**不是** repo 頁面上那排分頁裡的 Settings
   - 直接打開這個網址也可以：https://github.com/settings/personal-access-tokens
2. 左邊選單拉到最下面 **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**
3. 照下面填：

| 欄位 | 填什麼 | 為什麼 |
| --- | --- | --- |
| Token name | `club-cards` | 之後才知道這張鑰匙是做什麼的 |
| Expiration | 30 days | 鑰匙會自己過期，比較安全 |
| Repository access | **Only select repositories** → 選 `你的帳號/club-cards` | 只能開你 fork 的那個 repo |
| Repository permissions → **Contents** | **Read and write** | 讀 = 可以 clone / pull；寫 = 可以 push |

（Metadata 會自動變成 Read-only，不用管）

4. 按 **Generate token**，畫面上會出現一串 `github_pat_` 開頭的字，**按旁邊的複製按鈕，馬上複製**
   - 這串字**離開頁面就再也看不到了**，沒複製到只能重新產生一張
   - 等一下第一次 push 時才會用到，先存著：
     - **Windows**：按 `Win + V` 打開「剪貼簿歷程記錄」，剛剛複製的 Token 會在裡面，之後直接點它就能貼上
       - ⚠️ 第一次按 `Win + V` 會要你按「開啟」，開啟**之後**複製的東西才會被記住。所以建議在按 Generate token **之前**，先按一次 `Win + V` 把它開啟
     - **Mac**：先貼到「備忘錄」，push 完再刪掉

> ⚠️ Token 等於密碼：**不要傳給別人、不要截圖、不要 commit 進任何檔案**。
> 💡 為什麼只要 Contents 的讀寫？因為你只會推到**自己的 fork**；開 PR 是在網頁上做的，不需要 Token。權限給越少越安全，這叫「最小權限原則」。

### Step 4：Clone 你的 fork 到電腦

```bash
cd ~
git clone https://github.com/你的帳號/club-cards.git   # 把你的 fork 整個複製到電腦
cd club-cards
```

> ⚠️ 網址裡是**你的帳號**，不是 williamtsou。clone 錯的話，等一下 push 會出現 403。

✅ 檢查：clone 之後，`git remote -v` 會看到 `origin` 指向你的帳號。

### Step 5：接上老師的 repo

```bash
git remote add upstream https://github.com/williamtsou/club-cards.git
     # remote add：新增一個遠端，取名叫 upstream，指向老師的 repo
git remote -v
     # -v（verbose）：列出所有遠端和網址
```

✅ 檢查：看到 `origin`（你的帳號）和 `upstream`（williamtsou）各兩行（一行 fetch 拿東西用、一行 push 推東西用）。

### Step 6：開分支

```bash
git switch -c add-你的帳號
     # switch：切換分支；-c（create）：先建立再切過去
```

> 為什麼要開分支？main 是正式版，大家都不能直接改。你在自己的分支上改，改好再申請合併。
> 開完打 `git branch` 看看，前面有 `*` 的就是你現在所在的分支（也就是 HEAD 的位置）。

### Step 7：放進你的名片

```bash
code .      # 用 VS Code 打開「目前這個資料夾」（. 代表這裡）
```

在 VS Code 裡：

- [ ] 在 `cards/` 底下建一個資料夾，名稱是**你的帳號（全小寫）**
- [ ] 把你的 w10 名片（`index.html` + 頭像）放進去
  - 沒有名片？在終端機打 `cp -r cards/_template cards/你的帳號`（`cp` 複製，`-r` 連資料夾裡面的東西一起複製），再改裡面的文字
- [ ] 打開根目錄的 `index.html`，找到 `<ul class="members">`，**依帳號字母順序**插入一行：

```html
<li><a href="cards/你的帳號/index.html">你的名字</a></li>
```

- [ ] 在 `index.html` 上按右鍵 → **Open with Live Server**，點你的名字，確認進得去你的名片

### Step 8：存檔（commit）

```bash
git status
     # 看目前狀態：紅字 = 改了但還沒放進信封
git add .
     # add：放進信封（暫存區）；. = 這個資料夾裡所有改過的東西
git status
     # 變綠字 = 放進信封了
git commit -m "feat: 新增 你的名字 的名片"
     # commit：封口寄出，留下一筆紀錄；-m（message）：這筆紀錄的說明
     # feat: 開頭代表「新功能」，修錯用 fix:、改文件用 docs:
git log --oneline
     # 看所有紀錄，每筆一行
```

✅ 檢查：`git log` 最上面那行是你剛剛的訊息，而且有 `(HEAD -> add-你的帳號)`。

### Step 9：Push 到你的 fork

```bash
git push -u origin add-你的帳號
     # push：把紀錄推上 GitHub；origin：推到你的 fork
     # -u：記住「這個分支對應 origin 的 add-你的帳號」，之後只要打 git push 就好
```

**第一次 push 會要你登入，這時候用 Token：**

| 系統 | 會看到 | 怎麼做 |
| --- | --- | --- |
| Windows | 跳出 **Connect to GitHub** 視窗 | 選 **Token** 分頁，貼上 Token，按 Sign in |
| Mac | 終端機問 `Username` 和 `Password` | Username 打你的帳號；**Password 貼上 Token**（貼上時畫面不會顯示任何字，這是正常的，貼完直接按 Enter） |

登入一次之後，電腦會記住，之後 push 都不用再輸入。

### Step 10：開 PR（申請合併）

- [ ] 回到 `github.com/你的帳號/club-cards`，按黃色框的 **Compare & pull request**
- [ ] 確認方向：**左邊是 `williamtsou/club-cards` `main`，右邊是 `你的帳號/club-cards` `add-你的帳號`**
  - 意思是「把我的 add-你的帳號，合併進老師的 main」
- [ ] 照模板填好，在「對應的 Issue」寫 `Closes #你的Issue編號`
  - `Closes #編號`：PR 合併時，GitHub 會自動把那個 Issue 關掉
- [ ] 把自我檢查的框框勾一勾，按 **Create pull request**

### Step 11：等審核

- [ ] 去看看別人的 PR，在 **Files changed** 留一句話（問問題、給建議都可以），練習當審核者
  - 不知道要說什麼？看 [CONTRIBUTING 的「溝通方式」](../CONTRIBUTING.md#溝通方式)，有例句
- [ ] 老師審核通過、合併之後：
  - 你的 Issue 會**自動關閉**
  - 1～2 分鐘後，[名片牆](https://williamtsou.github.io/club-cards/) 上會出現你的名字 🎉

> **Issue 留言 vs PR 留言**
> - **Issue**：還沒動手前，討論「要不要做、做什麼、誰來做」。例如認領、問需求、回報進度
> - **PR**：已經寫好程式碼，討論「這段程式碼寫得對不對」。可以針對某一行留言、要求修改

### Step 12：收尾：同步最新版、刪掉用完的分支

PR 合併之後，這個分支的任務就完成了。**用完的分支要刪掉**，不然分支會越堆越多，分不清哪個還在用。

```bash
git switch main
     # 回到 main
git pull upstream main
     # pull = 從老師的 repo 抓最新的 main，合併進你電腦的 main（裡面已經有你的名片了）
git push origin main
     # 讓你的 fork 的 main 也跟上
git branch -D add-你的帳號
     # 刪掉電腦上的分支
     # 為什麼用大寫 -D？老師用 Squash 合併，會把你的 commit 壓成一筆新的，
     # Git 常常認不出「已經合併過了」，小寫 -d 會拒絕刪除（說 not fully merged），
     # 所以確認 PR 已經 Merged 之後，用大寫 -D 強制刪
git push origin --delete add-你的帳號
     # 刪掉 fork 上的分支
     # （如果你已經在 PR 頁面按過 Delete branch，這行會說找不到，沒關係）
git fetch origin --prune
     # 把電腦裡記著「fork 上還有這個分支」的舊紀錄也清掉
git branch -a
     # -a（all）：列出電腦上和遠端的所有分支，確認 add-你的帳號 都不見了
```

> ⚠️ **只刪已經合併的分支**。還沒合併的分支用 `-D` 刪掉，裡面的修改就不見了。
> 下一個任務：從最新的 main **開新分支**，不要沿用舊的。

---

## 4. 衝突練習（15 分鐘）

> **情境**：同事比你先合併了，而且改到跟你一樣的地方。輪到你的 PR 時，GitHub 說 **This branch has conflicts**。

### 4-1 先懂衝突是什麼

兩個人在同一本筆記的**同一行**寫了不同的字，Git 不敢亂猜，就把兩個版本都列出來請你選：

```
<<<<<<< HEAD
你的版本            ← HEAD = 你現在所在的分支，也就是「你這邊」
=======
別人的版本          ← 已經在正式版上的那個
>>>>>>> case1-theirs   ← 你合併進來的那個分支的名字
```

**原則：後合併的人，負責把自己的修改接到最新的正式版上。**

### 4-2 merge 是「誰合併進誰」？

**`git merge X` = 把 X 合併進「你現在所在的分支（HEAD）」。** 被改到的永遠是你現在站的那個分支，X 不會變。

| 指令 / 動作 | 誰 → 合併進 → 誰 |
| --- | --- |
| `git merge case1-theirs`（你站在 case1-mine） | case1-theirs → **case1-mine** |
| `git pull upstream main`（你站在 add-你的帳號） | 老師的 main → **add-你的帳號** |
| `git pull upstream main`（你站在 main） | 老師的 main → **你電腦的 main** |
| 老師在 GitHub 按 PR 的 Merge | 你 fork 上的 add-你的帳號 → **老師的 main** |

> 合併前先打 `git branch` 或 `git status`，確認自己站在哪個分支。

### 4-3 建練習場

```bash
cd ~/club-cards
bash scripts/conflict-lab.sh
     # 在 ~/conflict-lab 建一個獨立的練習 repo，不會影響 club-cards
cd ~/conflict-lab
```

### 4-4 三種情況

| 情況 | 指令 | 正確解法 |
| --- | --- | --- |
| 1. 兩個人都新增到同一個位置（**必做**） | `git switch case1-mine` → `git merge case1-theirs` | **兩個都留**，照字母排好 |
| 2. 同一行改成不同意思 | `git switch case2-mine` → `git merge case2-theirs` | **不要自己決定**，在 PR 留言問對方，討論後選一個 |
| 3. 別人改了規則（`cards/` 改名成 `people/`） | `git switch case3-mine` → `git merge case3-theirs` | **用別人的新版**，把你那行也改成 `people/` |

- `git switch case1-mine`：切到「你的」分支（HEAD 移到這裡）
- `git merge case1-theirs`：把「別人的」分支合併進來，改到同一行就會衝突

每一種都是同樣四步：

1. **決定內容**：打開有衝突的檔案（VS Code 會標顏色），決定最後要留什麼
   - 可以點上方的 **Accept Current**（留你的 = HEAD）/ **Accept Incoming**（留別人的）/ **Accept Both**（都留），或自己手動改
2. **刪掉標記**：`<<<<<<< HEAD`、`=======`、`>>>>>>> 分支名` 這三行一定要刪乾淨（用 Accept 按鈕的話會自動刪）
3. **存檔，告訴 Git 解好了**：
   ```bash
   git add .        # 把解好的檔案放進信封
   ```
4. **完成這次合併**：
   ```bash
   git commit       # 會跳出 VS Code，裡面已經寫好 Merge 訊息，直接關掉那個分頁就完成
   ```

✅ 檢查：`git status` 顯示乾淨；`git log --oneline` 最上面多了一筆 `Merge ...`。

### 4-5 先練習「反悔」

做情況 1 之前，先故意放棄一次：

```bash
git switch case1-mine
git merge case1-theirs
git merge --abort      # 放棄這次合併，回到 merge 之前的樣子
git status
```

✅ 檢查：`git status` 顯示一切乾淨，就像沒發生過。**解壞了隨時可以這樣重來。**

### 4-6 你的 PR 真的有衝突？

**常見情境**：你和小明同時開了 PR，兩個都還沒合併，而且都改到同一個地方。這時**還不會**顯示衝突。老師先合併了小明的 PR，你的 PR 才會變成 **This branch has conflicts**。

流程：

1. **你負責解**（你是後合併的那個）。先把老師最新的 main 合併進你的分支：
   ```bash
   cd ~/club-cards
   git switch add-你的帳號
   git pull upstream main     # 老師的 main → 你的分支，出現 CONFLICT
   ```
2. **看是哪一種情況**：
   - 兩邊都是新增（情況 1）、或是別人改了規則（情況 3）→ 直接照上面的四步解掉
   - 同一行改成不同意思（情況 2）→ **先別解**，到**你的 PR** 留言問清楚，例如：
     > @小明 你的 PR #5 把標題改成「名片牆」，我的改成「大家的自介」，衝突了。要用哪一個？還是合在一起？
     > @williamtsou 也麻煩老師決定一下 🙏
   - 等大家在 PR 留言裡決定好，再照決定的內容解
3. 解完推上去，PR 會自動更新，衝突提示消失：
   ```bash
   git push
   ```
4. 在 PR 留言「衝突已解決，請再看一次」，等老師審核、合併

> 討論要留在 **PR**（因為是在討論「這段程式碼要怎麼合」），不要私訊，之後其他人才看得到為什麼這樣改。

---

## 5. 課後任務

到 [Issues](https://github.com/williamtsou/club-cards/issues) 看任務，在想做的 Issue 底下**留言「我來做」**就是認領（任務 4 的功能 Issue 會由老師在課後開好）。

- 已經有人留言認領的，就換一個，避免兩個人做同一件事
- 有想做的功能但沒有 Issue？用「新功能」模板自己開一個，再留言認領

| 任務 | 內容 | 必做？ |
| --- | --- | --- |
| 1 | 名片 PR 被合併，名片牆上看得到你，並刪掉用完的分支 | ✅ |
| 2 | 在至少一個別人的 PR 留下有內容的審核留言 | ✅ |
| 3 | conflict-lab 三種情況都解過一次 | ✅ |
| 4 | 認領一個功能 Issue（搜尋框、Dark Mode、卡片排版…），完成並開 PR | 選做 |

### 每個任務的完整流程

不管做什麼任務，都是同一個循環。今天的名片就是走過一次這個循環：

```
認領 Issue → 同步 main → 開分支 → 改程式 → commit → push → 開 PR
   → 審核意見？改完再 commit + push → 有衝突？解掉再 push
   → 合併 → 收尾（同步 main、刪分支）→ 下一個任務
```

**① 認領 Issue（網頁）**

- 到 [Issues](https://github.com/williamtsou/club-cards/issues) 找一個想做的，在底下留言「我來做」
- 想做的事還沒有 Issue？用模板開一個新的
- 記下編號，例如 `#12`

**② 同步 main，開新分支**

```bash
cd ~/club-cards
git switch main                     # 回到 main
git pull upstream main              # 拿老師 repo 的最新版（別人合併的東西）
git push origin main                # 讓你的 fork 也跟上
git switch -c feat/12-search-bar    # 從最新的 main 開新分支：feat/編號-簡短描述
```

> 一定要先同步再開分支。從舊的 main 開分支，等於在舊版本上做事，之後很容易衝突。

**③ 改程式，邊做邊存檔**

```bash
code .                              # 用 VS Code 打開
```

- 用 Live Server 打開網頁，確認改的東西真的有效
- 做完一小段就存一次，不要全部做完才 commit：

```bash
git status                          # 看改了什麼
git diff                            # 看改了哪幾行（按 q 離開）
git add .
git commit -m "feat: 加入搜尋框"
```

> commit 訊息用類型開頭：`feat:` 新功能、`fix:` 修錯、`style:` 改樣式、`docs:` 改文件。

**④ Push，開 PR（網頁）**

```bash
git push -u origin feat/12-search-bar   # 這個分支第一次 push 要加 -u
```

- 到你的 fork 按 **Compare & pull request**
- 確認方向：`williamtsou/club-cards` `main` ← `你的帳號/club-cards` `feat/12-search-bar`
- 照模板填，寫 `Closes #12`

**⑤ 收到審核意見：在同一個分支上改**

老師或同學在 PR 留言要你修改，**不用重開 PR**，在同一個分支上改完再 push，PR 會自動更新：

```bash
git switch feat/12-search-bar       # 確認在這個任務的分支上
# ……照意見修改……
git add .
git commit -m "fix: 依審核意見調整搜尋框位置"
git push                            # 第二次之後直接 push 就好，不用再加 -u
```

改完在 PR 回覆留言（例如「已修改，請再看一次」），讓審核者知道。

**⑥ PR 出現 This branch has conflicts：解衝突**

```bash
git switch feat/12-search-bar
git pull upstream main              # 把最新的 main 合併進來 → 出現 CONFLICT
# ……照第 4 段的方法解衝突……
git add .
git commit
git push                            # PR 會自動更新，衝突提示消失
```

**⑦ 合併後：收尾**

```bash
git switch main
git pull upstream main              # main 裡現在有你的成果了
git push origin main
git branch -D feat/12-search-bar             # 刪電腦上的分支
git push origin --delete feat/12-search-bar  # 刪 fork 上的分支
git fetch origin --prune                     # 清掉舊紀錄
```

到 Issue 頁面確認它已經自動關閉，然後回到 ① 做下一個任務。

> **一個任務 = 一個 Issue = 一個分支 = 一個 PR。** 不要在同一個分支上做兩件事，也不要沿用已經合併的分支。

---

## 卡住了？

1. 先打 `git status`，它通常會告訴你下一步
2. 打 `pwd`，確認你在對的資料夾
3. 查 [速查表的「常見錯誤」](cheatsheet.md#常見錯誤)
4. 換貼紅色便利貼，或直接舉手

| 最常見的狀況 | 解法 |
| --- | --- |
| push 出現 403 / Permission denied | ① clone 到老師的 repo 了，要 clone **你自己的 fork**；② Token 沒選到你的 fork，或 Contents 不是 Read and write |
| push 出現 `Authentication failed` | 密碼欄打成 GitHub 密碼了，要貼 **Token**；Token 過期就重新產生一張 |
| 不小心在 main 上 commit | `git switch -c add-你的帳號`，commit 會跟著到新分支 |
| `git branch -d` 說 not fully merged | 分支是 Squash 合併的，確認 PR 已經 Merged 後改用 `-D` |
| 跳進一個奇怪的畫面出不來 | 按 `Esc`，打 `:q!` 再按 Enter；或按 `q` |
| 網頁上出現 `<<<<<<<` | 解衝突忘了刪標記，刪掉再 commit 一次 |
