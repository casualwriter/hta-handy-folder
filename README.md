# AmoyFolder

**English** | [繁體中文](README.zh-TW.md)

A tiny folder helper for Windows: one `.hta` file, no install, no dependencies.
Drop it into any folder and double-click it. Browse files, read Markdown,
preview HTML and images, open, edit or run files, and keep your own home page
of shortcuts.

## Features

- **Two panes**: file list on the left, viewer on the right.
- **Safe click**: clicking a name only *shows* the file. Nothing runs until you press **► run**.
- **Built-in viewers**: Markdown (headings, lists, tables, code, quotes, links, images),
  HTML (in a sandboxed frame), images, plain text. Other files show an info table.
- **Actions** (buttons in the right pane): view, edit, open, ► run, VS Code, reveal in Explorer, copy path.
- **Folder README**: when you enter a folder, its `README.md` is shown, plus tool buttons
  (CMD, PowerShell, Explorer, Git ...).
- **Home page**: your own Markdown page (`AmoyFolder.md`) with shortcut tiles.
  Add one with **+ shortcut**.
- **Configurable** with a plain `AmoyFolder.ini` (optional; built-in defaults are used when missing).
- Filter, sort by name or date, new file (date prefix `YYYYMMDD-`), new folder,
  breadcrumb path, Backspace = parent folder.

## Requirements

- Windows 7 / 10 / 11 with `mshta.exe` (built in).
- Runs in the IE engine (IE9 mode), so the page uses plain ES5 JavaScript and pure CSS.
- Optional: Notepad++ or VS Code (auto-detected), Python, Git.

## Quick start

1. Copy `AmoyFolder.hta` into the folder you want to manage.
2. Double-click it.

Or open any folder from the command line:

```bat
mshta.exe C:\path\to\AmoyFolder.hta "D:\some\folder"
```

Tip: put a shortcut to this command in `shell:sendto` to get "Send to > AmoyFolder".

## Using it

| Where | What |
|---|---|
| Top bar | Home icon, Root, Up, Refresh, Sort, + file, + folder, Filter |
| Top right | Settings (creates/edits `AmoyFolder.ini`), Edit this HTA, About |
| Left pane | Click a folder to enter it; click a file to show it |
| Right pane | The file (or folder README / home page) with its action buttons |

## Home page and shortcuts

Click the **home icon**. The page is `AmoyFolder.md` next to the `.hta`
(created the first time you press **edit** or **+ shortcut**).
Every list item that is a link shows as a tile:

```markdown
## Shortcuts

- [Projects](/projects)                 folder under the root
- [Notes](/docs/notes.md)               file under the root (shown, not run)
- [Downloads](C:\Users\me\Downloads)    any full path, spaces are fine
- [GitHub](https://github.com)          opens in the browser
- [Notepad](run:notepad.exe)            run: links run a command
```

## Settings (`AmoyFolder.ini`)

Click the settings icon to create the file from the defaults, edit it, then press **Refresh**.

```ini
[settings]
editor    =              ; empty = auto (Notepad++ > VS Code > notepad.exe)
browser   =              ; empty = default browser
root      =              ; empty = folder of the .hta
width     = 1024, 75%    ; px and/or % of screen, the smaller one is used
home      = AmoyFolder.md
start     = home         ; home | folder
hidden    = ^(\.git|node_modules|__pycache__)$

; extension = buttons in the right pane
[actions]
md, markdown = view, edit, open, copy
html, htm    = view, open, edit, copy
*            = open, reveal, copy

; extension = command; a ► run button appears for these
[run]
py  = python "{file}"
bat, cmd = "{file}"
hta, exe, msi, lnk, url = start      ; start = Windows default, no console

; buttons on each folder page
[tools]
CMD        = cmd.exe
Git status = cmd.exe /k git status
```

Placeholders: `{file}` full path, `{name}` file name, `{dir}` folder, `{root}` root folder.
See the built-in defaults at the top of the `<script>` in `AmoyFolder.hta` for the full list.

## Security notes

- An HTA runs with full user rights (like any program you start). Only use a copy you trust.
- Clicking never runs a file; **► run** and `run:` links do.
- HTML files are shown in an iframe with `application="no"`, so they get no HTA rights.
- `AmoyFolder.ini` and `AmoyFolder.md` are personal and are listed in `.gitignore`.

## Limits

- Markdown is a small subset (no HTML blocks, no footnotes).
- Text files are read as UTF-8.
- No delete or rename on purpose; use Explorer (**reveal**).

## License

MIT, see [LICENSE](LICENSE).
