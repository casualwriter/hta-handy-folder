# HandyFolder

[English](README.md) | **繁體中文**

Windows 上的小型資料夾助手：只有一個 `.hta` 檔，免安裝、無相依套件。
把它放進任何資料夾，雙擊就能用：瀏覽檔案、閱讀 Markdown、預覽 HTML 和圖片、
開啟／編輯／執行檔案，還能建立自己的捷徑首頁。

## 功能

- **雙欄介面**：左邊是檔案清單，右邊是預覽區。拖曳中間的分隔線可調整寬度（雙擊回到 40%）。
- **點擊安全**：點檔名只會「顯示」檔案，不會執行；要執行請按 **► run**。
- **內建預覽**：Markdown（標題、清單、表格、程式碼、引用、連結、圖片）、
  HTML（在沙盒框架中顯示）、圖片、純文字。其他檔案顯示資訊表。
- **動作按鈕**（在右邊）：view、edit、open、► run、VS Code、在檔案總管中顯示、複製路徑。
- **資料夾說明**：進入資料夾時顯示其 `README.md`，並附工具按鈕（CMD、PowerShell、檔案總管、Git…）。
- **首頁**：自己的 Markdown 頁面（`HandyFolder.md`），連結以方塊按鈕呈現，按 **+ shortcut** 新增捷徑。
- **可設定**：用簡單的 `HandyFolder.ini`（可省略，沒有時使用內建預設值）。
- 篩選、依名稱或日期排序、新增檔案（自動加日期前綴 `YYYYMMDD-`）、新增資料夾、
  路徑導覽列、Backspace 回上一層。

## 系統需求

- Windows 7 / 10 / 11，內建 `mshta.exe` 即可。
- 以 IE 引擎（IE9 模式）執行，因此只用 ES5 JavaScript 和純 CSS。
- 選用：Notepad++ 或 VS Code（自動偵測）、Python、Git。

## 快速開始

1. 把 `HandyFolder.hta` 複製到要管理的資料夾。
2. 雙擊開啟。

也可以從命令列開啟任意資料夾：

```bat
mshta.exe C:\path\to\HandyFolder.hta "D:\some\folder"
```

小技巧：把這個指令的捷徑放到 `shell:sendto`，就能在右鍵選單用「傳送到 > HandyFolder」。

## 使用方式

| 位置 | 內容 |
|---|---|
| 頂部列 | 首頁圖示、Root、Up、Refresh、排序、+ file、+ folder、篩選 |
| 右上角 | 設定（建立／編輯 `HandyFolder.ini`）、編輯本 HTA、關於 |
| 左欄 | 點資料夾進入；點檔案顯示 |
| 右欄 | 檔案內容（或資料夾 README／首頁）與動作按鈕 |

## 首頁與捷徑

點 **首頁圖示**。首頁內容是 `.hta` 旁的 `HandyFolder.md`
（第一次按 **edit** 或 **+ shortcut** 時才建立）。清單中的連結會顯示成方塊按鈕：

```markdown
## Shortcuts

- [Projects](/projects)                 根目錄下的資料夾
- [Notes](/docs/notes.md)               根目錄下的檔案（只顯示，不執行）
- [Downloads](C:\Users\me\Downloads)    任意完整路徑，可含空格
- [GitHub](https://github.com)          用瀏覽器開啟
- [Notepad](run:notepad.exe)            run: 連結會執行指令
```

## 設定（`HandyFolder.ini`）

點設定圖示會用預設值建立檔案並開啟編輯，改完按 **Refresh** 生效。

```ini
[settings]
editor    =              ; 空白 = 自動（Notepad++ > VS Code > notepad.exe）
browser   =              ; 空白 = 預設瀏覽器
root      =              ; 空白 = .hta 所在資料夾
width     = 1024, 75%    ; 像素和／或螢幕百分比，取較小者
split     = 40           ; 左欄寬度（%），拖曳分隔線後自動儲存
home      = HandyFolder.md
start     = home         ; home | folder
hidden    = ^(\.git|node_modules|__pycache__)$

; 副檔名 = 右欄按鈕
[actions]
md, markdown = view, edit, open, copy
html, htm    = view, open, edit, copy
*            = open, reveal, copy

; 副檔名 = 指令；有設定的類型會出現 ► run 按鈕
[run]
py  = python "{file}"
bat, cmd = "{file}"
hta, exe, msi, lnk, url = start      ; start = Windows 預設方式開啟，不開命令視窗

; 每個資料夾頁面上的工具按鈕
[tools]
CMD        = cmd.exe
Git status = cmd.exe /k git status
```

替換符號：`{file}` 完整路徑、`{name}` 檔名、`{dir}` 所在資料夾、`{root}` 根目錄。
完整預設值請看 `HandyFolder.hta` 中 `<script>` 開頭的設定區。

## 安全說明

- HTA 以使用者完整權限執行（和一般程式一樣），請只使用你信任的版本。
- 點擊不會執行檔案；只有 **► run** 和 `run:` 連結會執行。
- HTML 檔在 `application="no"` 的 iframe 中顯示，不具 HTA 權限。
- `HandyFolder.ini` 和 `HandyFolder.md` 屬個人設定，已列入 `.gitignore`。

## 限制

- 只支援常用的 Markdown 語法（不支援 HTML 區塊、註腳）。
- 文字檔一律以 UTF-8 讀取。
- 刻意不提供刪除與改名，請用檔案總管（**reveal**）。

## 授權

MIT，詳見 [LICENSE](LICENSE)。
