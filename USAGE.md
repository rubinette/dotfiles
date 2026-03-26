# Dotfiles 使用說明

這份文件說明如何在新機器上安裝、日常更新與清理這份 dotfiles。

## 1. 前置需求

- 作業系統：macOS（Apple Silicon / Intel 皆可）
- 已安裝：`git`、`make`、[Homebrew](https://brew.sh/)
- 建議先備份既有設定檔（例如 `~/.vimrc`、`~/.gitconfig`）

## 2. 新機安裝

1. 取得程式碼

```bash
git clone git@github.com:<your-account>/dotfiles.git ~/Code/dotfiles
cd ~/Code/dotfiles
```

2. 安裝 Homebrew 套件

```bash
brew bundle --file Brewfile
```

3. 建立設定檔 symlink

```bash
make
```

4. 設定 fish 為預設 shell

```bash
which fish
chsh -s "$(which fish)"
```

5. 首次啟動 Neovim 安裝 plugin（`lazy.nvim` 會自動處理）

```bash
nvim
```

## 3. 會被 `make` 連結的主要檔案

- fish: `~/.config/fish/config.fish`、`~/.config/fish/functions/`
- Neovim: `~/.config/nvim/init.lua`
- Vim: `~/.vimrc`
- tmux: `~/.tmux.conf`、`~/.tmux/tmux-dark.conf`、`~/.tmux/tmux-light.conf`
- Git/Tig: `~/.gitconfig`、`~/.tigrc`
- Ghostty: `~/.config/ghostty/config`
- Zed: `~/.config/zed/settings.json`、`keymap.json`、`tasks.json`
- Cursor: `~/Library/Application Support/Cursor/User/settings.json`、`keybindings.json`

## 4. 日常使用與更新

1. 修改 `~/Code/dotfiles` 內設定檔
2. 重新同步

```bash
cd ~/Code/dotfiles
make
```

3. 重新載入常見工具

- fish: `exec fish`
- tmux: `tmux source-file ~/.tmux.conf`
- nvim: 重開或 `:Lazy sync`

## 5. 清理（移除 symlink）

```bash
cd ~/Code/dotfiles
make clean
```

注意：`make clean` 會直接刪除上述路徑上的檔案。若該路徑不是 symlink，而是你手動建立的真實檔案，也會被刪掉。請先自行備份。

## 6. 快捷鍵速查

### 6.1 Tmux（`prefix = Ctrl-f`）

- `prefix + v`：垂直分割（左右 pane）
- `prefix + s`：水平分割（上下 pane）
- `prefix + h/j/k/l`：切換 pane
- `prefix + H/J/K/L`：調整 pane 大小
- `prefix + x`：關閉目前 pane（不需確認）
- `prefix + r`：重新載入 `~/.tmux.conf`
- `prefix + C-s`：切換 pane 同步輸入
- `Ctrl+Shift+Left/Right`：左右交換 window（不需 prefix）

### 6.2 Ghostty（映射到 tmux）

- `Cmd+d`：開左右分割
- `Cmd+Shift+d`：開上下分割
- `Cmd+w`：關閉目前 pane
- `Cmd+h/j/k/l`：切換 pane
- `Cmd+←/→/↑/↓`：調整 pane 大小
- `Cmd+t`：新開 tmux window
- `Cmd+1..9`：切換到對應 tmux window

### 6.3 Neovim（`<Leader> = ,`）

- `,w`：儲存
- `,q`：離開
- `,n`：切換檔案樹
- `,f`：在檔案樹定位目前檔案
- `Ctrl+p`：`fzf` 搜尋 git tracked files
- `Ctrl+b`：`fzf` 搜尋所有檔案
- `Ctrl+g`：文件符號（LSP symbols）
- `Ctrl+n / Ctrl+m`：quickfix 下一筆 / 上一筆
- `,a`：關閉 quickfix
- `Ctrl+h/j/k/l`：視窗切換（含 terminal mode）
- `jj` 或 `jk`：insert mode 離開到 normal
- `gd / gr / gi / gD`：LSP 定義、參考、實作、宣告
- `,rn`：LSP rename
- `,b`：Go build（測試檔會走 `GoTestCompile`）
- `,<space>`：清除搜尋高亮

### 6.4 Cursor（VS Code Vim）

- `,w`：儲存
- `,q`：關閉目前編輯分頁
- `,n` 或 `,f`：開啟檔案總管
- `Ctrl+n / Ctrl+m`：下一個 / 上一個錯誤
- `,a`：打開 Problems
- `Ctrl+h/j/k/l`：在編輯 pane 間切換
- `Ctrl+t`：切換整合終端機
- `Ctrl+g`：Go to Symbol
- `Ctrl+;`：Go to Line
- `Cmd+1..9`：切換編輯分頁索引

### 6.5 Zed（Vim mode）

- `,w`：儲存
- `,q`：關閉目前分頁
- `,n`：切換 Project Panel
- `Cmd+Shift+g`：切換 Git Panel
- `Ctrl+h/j/k/l`：切換 pane
- `Ctrl+w s / Ctrl+w v`：分割 pane（下 / 右）
- `Ctrl+g`：切換 Outline
- `g r`：找 references
- `g n`：rename symbol
- `Shift+k`：hover 文件
- `,b`：執行 `Go Build` task
- `,a`：執行 `Go Alternative` task（Go 檔與測試檔互切）

## 7. Helper 指令（fish functions）

這些 helper 來自 `fish/functions/*.fish`，啟用 fish 後可直接在 shell 使用。

### 7.1 導航與檔案

- `c`：從 `~/Code/*` 與 `~/Code/ps/*` 模糊搜尋目錄並 `cd`
- `d`：在目前目錄下模糊搜尋子目錄並 `cd`
- `cdr`：跳到目前 git repo root（`cd (git rev-parse --show-toplevel)`）
- `f`：模糊搜尋檔案後用 `nvim` 開啟
- `icloud`：快速切到 iCloud Drive 目錄

### 7.2 Git / GitHub workflow

- `b`：模糊搜尋本地 branch 並 checkout
- `co`：checkout 到遠端預設分支（例如 `main`）
- `po`：`git pull origin <current-branch>`
- `gp`：`git push`
- `gf`：`git push -f`
- `sq`：從與預設分支共同祖先點做互動式 rebase
- `tigs`：`tig status`
- `hb`：在瀏覽器開啟目前 repo（`gh repo view --web`）
- `hc`：在瀏覽器開啟目前 PR（`gh pr view --web`）
- `ghpr`：push 當前分支並開 `gh pr create --web`
- `pushtag <version> <comment>`：切到預設分支、pull 後建立並 push annotated tag
- `ghdel`：切到預設分支，刪除本地所有名稱含 `fatih` 的 branch（高風險，使用前請先確認）

### 7.3 開發工具整合

- `vim`：等價 `nvim`
- `bt`：`go build ./... && go test ./...`
- `usego <version>`：切換 Go 版本；若缺少會先安裝 `go<version>`
- `nvm use <version>`：fish 版 `nvm`（含 `.nvmrc` 支援）
- `vtctlclient ...`：自動帶 `--server localhost:15999 --alsologtostderr`
- `killvitess`：清理本機 vitess 相關 process 與 pid 檔
- `tailscale ...`：呼叫 App 內建 Tailscale binary

### 7.4 AI / 通知整合

- `claude ...`：若在 git repo 子目錄，先切到 repo root 再執行 `claude`
- `amp ...`：若在 git repo 子目錄，先切到 repo root 再執行 `amp --ide`
- `awtrix_done`：依上一個指令成功/失敗，送 DONE/ERROR 通知到 AWTRIX 3
- `alacritty-theme <name>`：修改 Alacritty 色彩錨點（`~/.config/alacritty/color.yml`）

### 7.5 Prompt 與鍵盤

- `fish_prompt`：客製 prompt（目前路徑 + git 狀態）
- `fish_user_key_bindings`：載入 FZF keybindings，並釋放 `Ctrl+t` 給編輯器/終端整合使用
- `fzf_key_bindings`：FZF widgets（`Ctrl+t` 檔案、`Ctrl+r` 歷史、`Alt+c` 切目錄）

## 8. 常見問題

### Q1. `make` 後設定沒有生效

- 先檢查目標檔是否本來就存在。`Makefile` 使用 `[ -f ... ] || ln -s ...`，如果檔案已存在就不會覆蓋。
- 可先手動搬走舊檔，再執行 `make`。

### Q2. fish 啟動報 `~/.private.fish` 找不到

`config.fish` 會 `source ~/.private.fish`。如果你沒有私人設定，請建立空檔：

```bash
touch ~/.private.fish
```

### Q3. tmux plugin 沒有安裝

這份設定使用 TPM（tmux plugin manager）。請先安裝 TPM，然後在 tmux 內按 `prefix + I` 安裝 plugin。

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

## 9. 鍵盤韌體

- Glove80：參考 `zmk/readme.md`
- Kinesis Advantage 2：參考 `qmk/advantage-2/readme.md`
- Mode Encore：參考 `qmk/mode-encore/readme.md`
- Halcyon Elora：參考 `qmk/halycon-elora/readme.md`
