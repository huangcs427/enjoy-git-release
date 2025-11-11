<p align="center">
  <img width="150" src="https://enjoygit.com/images/logo.png">
</p>

[English](./README.md) | [簡體中文](./README.zh-CN.md) | 繁體中文

# [官網](https://enjoygit.com) · [私隱政策](https://enjoygit.com/privacyPolicy.html)

# Enjoy Git - 簡易高效的 Git 客戶端

Enjoy Git 是一款基於 **Electron、Vue 3 與 TypeScript** 的現代化 Git 圖形客戶端。三欄布局將導覽、檔案列表與 diff / 提交區集中呈現，把日常 Git 操作視覺化，同時保留行級暫存、提交圖、衝突解決等進階能力。

---

## 支援平台

| 平台 | 版本 / 形態 | 架構 | Git |
|------|-------------|------|-----|
| **Windows** | Windows 10、Windows 11 | x64、arm64 | **內建**（dugite-native），開箱即用 |
| **macOS** | macOS 10.15 及以上 | x64（Intel 舊款 Mac）、arm64（Apple Silicon M1 及以後） | **內建**（dugite-native），開箱即用 |
| **Linux** | Debian 系（`.deb`）、Red Hat 系（`.rpm`） | x64、arm64 | 使用**系統環境**中的 Git，需預先安裝 |

> Windows / macOS 安裝後即可使用，無需單獨安裝 Git。Linux 請先安裝系統 Git（如 `sudo apt install git`），並確保終端可執行 `git` 指令。

---

## 介面預覽

<p align="center">
  <a href="docs/images/enjoy-git.gif">
    <img src="docs/images/enjoy-git.gif" alt="Enjoy Git 應用演示" width="920" />
  </a>
</p>

---

## 支援的語言

介面可在選單 **Language** 中切換：

- **簡體中文**
- **繁體中文**
- **English**

---

## 支援的主題

在選單 **View（檢視）** 中切換：

- **深色模式**
- **淺色模式**

---

## 功能

| 功能 | 說明 |
|------|------|
| **多倉庫** | 頂部標籤同時開啟多個本機倉庫，各自保留分支、暫存與檢視狀態 |
| **複製與接入** | 複製 HTTP / SSH 遠端倉庫、新增本機專案；支援遞迴複製子模組 |
| **工作區** | 已暫存 / 未暫存分區；列表與檔案樹切換；批次暫存；檔案右鍵（歷史、Blame、貯藏、外部開啟等） |
| **差異審閱** | 統一 / 雙欄 diff；語法高亮；**行級暫存**與丟棄；未變更程式碼預設摺疊，**可向上/向下展開指定上下文**或一次性展開全部；圖片 diff |
| **提交** | 摘要與詳情；Amend；跳過鉤子；提交並推送；**智能生成提交訊息** |
| **提交歷史** | 視覺化提交圖；搜尋篩選；變更列表 / 檔案樹；多選 Cherry-pick、Squash、Revert |
| **分支與遠端** | 檢出、合併、變基、追蹤、重新命名；可設定推送選項；多遠端管理 |
| **標籤與貯藏** | 標籤建立與檢出；貯藏套用 / 彈出 / 刪除 |
| **檔案追溯** | 單檔歷史視窗；按行審閱（Blame） |
| **衝突解決** | 合併、變基、拉取等場景的可視化衝突編輯；接受目前 / 傳入 / 雙方 |
| **選單與快捷操作** | File / View / Repository / Help 完整選單；Fetch、Pull、Push 等快捷鍵 |
| **設定** | 外部開啟程式、SSH 金鑰、智能生成提交訊息；日誌與設定目錄 |

---

## 下載與安裝

前往 [GitHub Release](https://github.com/huangcs427/enjoy-git-release/releases) 下載對應平台與架構的安裝套件，按精靈安裝即可。

---

## 常見問題

### 與其他 Git 客戶端相比有什麼優勢？

介面直觀，在**衝突解決**、**部分提交（行級暫存）**和**檔案歷史**方面更易上手，兼顧新手與進階用戶。

### 會監察我的倉庫資料嗎？

**不會。** Git 操作、憑證與日誌主要保存在本機，不會上傳讀取您的提交或倉庫內容。基礎使用統計請參閱[私隱政策](https://enjoygit.com/privacyPolicy.html)。

### 如何反映問題？

- [GitHub Issue](https://github.com/huangcs427/enjoy-git-release/issues)
- [huangcs427@163.com](mailto:huangcs427@163.com)

---

## dugite-native 調用源程式碼

```ts
/**
 * 本軟件內建的 Git 使用 dugite-native
 * https://github.com/desktop/dugite-native
 */
import { spawn } from 'child_process';
import * as fs from 'fs-extra';
import * as path from 'path';

const getWin32GitSubfolder = (arch?: string): string => {
  const archRes = arch || process.arch
  if (archRes === 'x64') return 'mingw64'
  if (archRes === 'arm64') return 'clangarm64'
  return 'mingw32'
}

interface TObjectValue { [key: string]: any }

const getGitPathAndGitEnv = (envTemp?: TObjectValue) => {
  let env = { ...process.env, ...(envTemp || {}) }
  const gitFolder = path.join(__dirname, 'git')
  let gitPath = ''
  if (process.platform === 'win32') {
    const win32GitSubfolder = getWin32GitSubfolder()
    gitPath = path.join(gitFolder, 'cmd', 'git.exe')
    env.GIT_EXEC_PATH = path.join(gitFolder, win32GitSubfolder, 'libexec', 'git-core')
    env.PATH = `${gitFolder}\\${win32GitSubfolder}\\bin;${gitFolder}\\usr\\bin;${env.PATH ?? ''}`
  } else {
    gitPath = path.join(gitFolder, 'bin', 'git')
    env.GIT_CONFIG_SYSTEM = path.join(gitFolder, 'etc', 'gitconfig')
    env.GIT_EXEC_PATH = path.join(gitFolder, 'libexec', 'git-core')
    env.GIT_TEMPLATE_DIR = path.join(gitFolder, 'share', 'git-core', 'templates')
  }
  if (!fs.existsSync(gitPath)) {
    env = { ...envTemp }
    gitPath = 'git'
  }
  return { env, gitPath }
}

export const git = (args: string[], options: TObjectValue) => {
  if (!options) options = {}
  const { gitPath, env } = getGitPathAndGitEnv(options.env)
  options.env = env
  return spawn(gitPath, args, options)
}
```

---

## 意見與建議

- [GitHub Issue](https://github.com/huangcs427/enjoy-git-release/issues)
- [huangcs427@163.com](mailto:huangcs427@163.com)
- [私隱政策](https://enjoygit.com/privacyPolicy.html)
