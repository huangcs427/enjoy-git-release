<p align="center">
  <img width="150" src="https://enjoygit.com/images/logo.png">
</p>

[English](./README.en.md) | [简体中文](./README.md) | [繁體中文](./README.zh-HK.md)

# [Website](https://enjoygit.com) · [Privacy Policy](https://enjoygit.com/privacyPolicy.html)

# Enjoy Git - Simple & Efficient Git Client

Enjoy Git is a modern Git GUI built with **Electron, Vue 3, and TypeScript**. A three-column layout brings navigation, file lists, and diff / commit areas together—visualizing everyday Git work while keeping line-level staging, commit graphs, and conflict resolution within reach.

---

## Supported Platforms

| Platform | Version / Format | Architectures | Git |
|----------|------------------|---------------|-----|
| **Windows** | Windows 10, Windows 11 | x64, arm64 | **Bundled** (dugite-native), ready to use |
| **macOS** | macOS 10.15 and later | x64 (older Intel Macs), arm64 (Apple Silicon M1 and later) | **Bundled** (dugite-native), ready to use |
| **Linux** | Debian-based (`.deb`), Red Hat-based (`.rpm`) | x64, arm64 | **System Git** required |

> On Windows and macOS, install the app and start using Git—no separate Git install. On Linux, install system Git first (e.g. `sudo apt install git`) and ensure `git` works in your terminal.

---

## Preview

<p align="center">
  <a href="docs/images/enjoy-git.gif">
    <img src="docs/images/enjoy-git.gif" alt="Enjoy Git demo" width="920" />
  </a>
</p>

---

## Supported Languages

Switch via menu **Language**:

- **Simplified Chinese**
- **Traditional Chinese**
- **English**

---

## Supported Themes

Switch via menu **View**:

- **Dark mode**
- **Light mode**

---

## Features

| Feature | Description |
|---------|-------------|
| **Multi-repository** | Open multiple local repos in tabs; each keeps its own branch, staging, and view state |
| **Clone & onboarding** | Clone over HTTP / SSH, add local repos; recursive submodule clone |
| **Working directory** | Staged / unstaged sections; list and file tree views; batch stage; file context menus (history, Blame, stash, open externally, etc.) |
| **Diff review** | Unified / side-by-side diff; syntax highlighting; **line-level stage** and discard; unchanged code folded by default, **expand context up/down** or reveal all at once; image diff |
| **Commit** | Summary and description; Amend; skip hooks; commit and push; **smart commit message generation** |
| **Commit history** | Visual commit graph; search; change list / file tree; multi-select Cherry-pick, Squash, Revert |
| **Branches & remotes** | Checkout, merge, rebase, tracking, rename; configurable push; multiple remotes |
| **Tags & stash** | Create and checkout tags; stash apply / pop / delete |
| **File history** | Per-file history window; line-by-line Blame |
| **Conflict resolution** | Visual conflict editor for merge, rebase, pull, etc.; accept current / incoming / both |
| **Menus & shortcuts** | Full File / View / Repository / Help menus; Fetch, Pull, Push shortcuts |
| **Settings** | External programs, SSH keys, smart commit message generation; logs and config directories |

---

## Download & Install

Download the installer for your platform and architecture from [GitHub Releases](https://github.com/huangcs427/enjoy-git-release/releases).

---

## FAQ

### What makes Enjoy Git different?

An intuitive UI with strong support for **conflict resolution**, **partial commits (line-level staging)**, and **file history**—accessible for beginners and power users alike.

### Does Enjoy Git monitor my repositories?

**No.** Git operations, credentials, and logs stay on your machine; your commits and repo contents are not uploaded for reading. See the [privacy policy](https://enjoygit.com/privacyPolicy.html) for basic usage statistics.

### How do I report issues?

- [GitHub Issues](https://github.com/huangcs427/enjoy-git-release/issues)
- [huangcs427@163.com](mailto:huangcs427@163.com)

---

## dugite-native Source Code

```ts
/**
 * Bundled Git uses dugite-native
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

## Feedback

- [GitHub Issues](https://github.com/huangcs427/enjoy-git-release/issues)
- [huangcs427@163.com](mailto:huangcs427@163.com)
- [Privacy Policy](https://enjoygit.com/privacyPolicy.html)
