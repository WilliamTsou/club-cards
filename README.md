# 專案組名片牆

每位專案組成員的自介名片都放在這裡。這個 repo 也是我們練習 Git 與多人協作的地方。

- 網站：https://williamtsou.github.io/club-cards/
- 第一次參加：照著 [學生講義](docs/handout.md) 做
- 參與方式、溝通方式、遇到衝突：[CONTRIBUTING.md](CONTRIBUTING.md)
- 指令和快捷鍵忘了：[速查表](docs/cheatsheet.md)
- Git 動畫圖解：https://williamtsou.github.io/club-cards/docs/git-visual.html

## 需要的工具

| 工具 | Windows | Mac |
| --- | --- | --- |
| Git | 到 https://git-scm.com/downloads/win 下載，會一起裝好 **Git Bash** | 終端機打 `git --version`，照提示安裝 |
| 終端機 | **Git Bash**（不要用 PowerShell） | 內建的「終端機」 |
| 編輯器 | [VS Code](https://code.visualstudio.com) + Live Server 延伸模組 | 同左 |

> 這個 repo 的 `bash scripts/xxx.sh` 要在 **Git Bash**（Windows）或終端機（Mac）執行。
> 在 PowerShell 打 `bash` 會叫到別的程式，跑不起來。

## 資料夾結構

```
club-cards/
├── index.html            # 目錄頁（名片牆）
├── cards/
│   ├── _template/        # 名片範本，還沒做 w10 名片的人可以複製這個
│   └── <GitHub帳號>/      # 每人一個資料夾
├── docs/
│   ├── handout.md        # 學生講義（上課照著做）
│   ├── cheatsheet.md     # 指令與快捷鍵速查表
│   └── git-visual.html   # Git 動畫圖解
├── scripts/
│   ├── terminal-quest.sh # 終端機尋寶練習
│   ├── conflict-lab.sh   # 三種衝突練習場
│   └── setup-repo.sh     # 維護者用：一次設定 repo
└── .github/              # Issue 模板、PR 模板、CODEOWNERS
```

## 練習用腳本

在 Git Bash（Mac 用終端機）執行，不會動到這個 repo：

```bash
bash scripts/terminal-quest.sh   # 終端機尋寶，建在 ~/terminal-quest（~ 是你的家目錄）
bash scripts/conflict-lab.sh     # 衝突練習場，建在 ~/conflict-lab
```

## 在自己電腦上預覽

1. 用 VS Code 打開這個資料夾
2. 在 `index.html` 上按右鍵 → **Open with Live Server**

## 開發流程

**開 Issue → Fork → 開分支 → commit → push 到自己的 fork → 開 PR → 審核 → 合併 → 刪分支**
細節都寫在 [CONTRIBUTING.md](CONTRIBUTING.md)。
