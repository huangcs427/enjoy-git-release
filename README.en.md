<p align="center">
  <img width="150" src="https://enjoygit.com/images/logo.png">
</p>

English | [简体中文](./README.md)

# [Official Website](https://enjoygit.com)

# Enjoy Git - A Simple and Efficient Git Client
A modern Git client tool with an intuitive user interface, built with Electron, Vue3, and TypeScript. 

## Supported Platforms

### Currently Supported
- **Windows**
  - Windows 10 / Windows 11 (arm64 / x64)

### Coming Soon
- **macOS**: Release delayed due to Apple Developer Account issues.
- **Linux**: Still in testing.

## Interface Preview

### Home Page
Get a clear overview of the repository status at a glance.
![Home Page](https://enjoygit.com/images/index.png?timestamp=1694500000000)

### Powerful Diff Comparison
> Supports two-column display of file differences, highlights modified content line by line, and allows viewing substantive changes while ignoring whitespace. It works not only with code files but also enables diff comparison for images.
- Two-column display of file diffs for clear comparison
- Highlights modified lines for intuitive change identification
- Supports image diffs for easy visual change comparison
- Allows expanding context lines to view complete code changes
- Ignores whitespace differences to focus on substantive code changes
- Enables editing workspace files directly in the diff comparison interface
- Uses virtual list to load diff files and only loads content in the currently visible area for improved performance
![Powerful Diff Comparison](https://enjoygit.com/images/diff.png)

### Intelligent Conflict Resolution
> When code conflicts occur (stash apply conflicts, merge conflicts, cherry-pick conflicts, rebase conflicts, rollback conflicts), an intuitive conflict resolution interface is provided. It displays conflicting content in blocks and supports selecting to use the current version, incoming version, or retain both changes for easy resolution of complex conflicts.
- Displays conflicting files in blocks for clear problem localization
- Flexibly selects to retain current/incoming version or both changes
- Quickly jumps to conflict locations to improve resolution efficiency
- Supports skipping current conflicts or terminating the conflict resolution process
![Intelligent Conflict Resolution](https://enjoygit.com/images/conflict.png)

### Efficient Branch and Commit Management
> Easily manage multiple branches, create new branches from any starting point, view detailed commit history, and use cherry-pick to apply specific commits to the current branch.
- Create new branches from HEAD, tags, commits, or remote branches
- View all branch lists for easy checkout and switching
- Supports cherry-pick for selective application of commits
- View commit file trees to understand the complete code state
- Virtual list and pagination loading for fast loading speed and high performance
![Efficient Branch and Commit Management](https://enjoygit.com/images/commit.png)

### File History
> Supports viewing the history of specific files in specific branches for easy code change tracing.
- View the history of specific files
- Supports branch switching for filtering
- Supports keyword search for filtering
![File History](https://enjoygit.com/images/history.png)

### Line-by-Line File Review
> Uses the `git blame` function to view the author, commit information, and timestamp of each line of code in a file. It helps teams track code change history, identify the responsible person for each line of code, and facilitate code review and problem tracing.
- View the history of specific files
- Supports branch switching for filtering
- Supports keyword search for filtering
![Line-by-Line File Review](https://enjoygit.com/images/blame.png)

## Core Feature Highlights

### 🔍 Workspace and Staging Area Management
- Real-time display of changed file lists in the workspace/staging area, clearly distinguishing between "Untracked", "Modified", and "Staged" statuses
- Supports single file diff viewing, with precise selection of **specific diff lines** for operations:
  - Discard selected changes (abandon local modifications)
  - Add to staging area (supports partial commits)
  - Remove from staging area (unstage)

### 🚀 Remote Repository Interaction
- **Fetch/Pull**:
  - Supports switching between normal pull and `rebase pull` modes
  - Automatically stashes local changes before pulling and applies them after pulling to avoid conflicts
- **Push**:
  - Supports pushing to specified branches, with optional "Force Push" (requires secondary confirmation)
  - Automatically verifies remote permissions and branch protection rules before pushing

### ✍️ Commit and History Management
- **Commit**:
  - One-click commit of staged files, supporting standardized information with "Title + Detailed Description"
  - Supports `git amend` to modify the last commit (retains continuity of change history)
  - View which files were modified in a single commit or multiple consecutive commits, and check the complete file tree of the commit state
- **Commit History Tracing**:
  - View commit history from multiple dimensions (current branch/local branches/remote branches)
  - Supports operations such as **deleting commits**, "Soft Reset", "Hard Reset", and "Undo Commit"
  - Cherry-pick single or multiple commits to the target branch for precise change migration

### 🌿 Branch and Tag Management
- **Branch Operations**:
  - Quickly create new branches from "current HEAD/tag/specific commit/remote branch"
  - Visual branch list, supporting checkout and deletion of local/remote branches
  - Multi-remote management: Add/edit/delete remote repository configurations, supporting http/ssh protocols
- **Tag Features**:
  - Create new tags (including annotated tags), check out historical tag versions, and delete tags
  - Supports filtering and searching

### 📦 Stash Management
- One-click stash of all changes, or precisely select specific files to stash (completely retains staging area status)
- Supports applying specified stashes to the current workspace, selecting and deleting one/multiple stashes, and batch clearing all stash records

### 🔀 Conflict Resolution
- Visual block display of conflicting files, supporting paragraph-by-paragraph selection of "current version/incoming version/retain both changes"
- Quick operations: One-click jump to the next conflict location, temporarily skip current conflicts, terminate the conflict resolution process
- VSCode-like conflict editor for intuitive difference comparison and reduced manual merging complexity

### 📄 File Diff and History Tracing
- **Enhanced Diff Features**:
  - Supports image diff preview
  - Switch between two-column comparison/single-line highlight modes, with optional whitespace ignore
  - Highlights modified lines to clearly distinguish between "added/deleted/modified" lines
  - Supports code highlighting
  - Expand specific context-related lines or one-click expand all lines
- **File History and Traceability**:
  - Separate window displays the complete commit history of files, including diff details for each change
  - Line-by-line review: View the last modifier and corresponding commit of each line in the file
  - Supports resetting files to specific commit versions or opening "latest version/current modified version" with external programs
- **Commit Content Analysis**:
  - View file diff lists and tree structures of single/multiple consecutive commits
  - Displays the complete file tree of a commit (snapshot of all files at current HEAD)

### 🛠️ Basic Repository Management
- Built-in Git environment, no additional dependencies required
- Supports adding existing local Git repositories or cloning remote repositories via http/ssh
- Automatically verifies permissions (ssh keys/username and password) during cloning, with detailed error prompts if failed

## Download and Installation
Visit [release](https://github.com/huangcs427/enjoy-git-release/releases) to download the corresponding installation package and follow the guide to complete the installation.

## Usage Help
- Encounter issues? Submit an [Issue](https://github.com/huangcs427/enjoy-git-release/issues) to provide feedback.

## dugite-native Call Source Code
```ts
/**
 * 本软件内置的Git使用了dugite-native
 * 项目地址：https://github.com/desktop/dugite-native
 * 本文件包含了获取git路径和环境变量的函数，以及导出git命令函数的代码。
 */
import { spawn } from 'child_process';
import * as fs from 'fs-extra';
import * as path from 'path';

// 获取windows的git实例子目录，根据架构返回不同的目录
const getWin32GitSubfolder = (arch?: string): string => {
  const archRes = arch || process.arch
  if (archRes === 'x64') {
    return 'mingw64'
  } else if (archRes === 'arm64') {
    return 'clangarm64'
  } else {
    return 'mingw32'
  }
}

// 获取git路径和环境变量
interface TObjectValue {
  [key: string]: any
}
const getGitPathAndGitEnv = (envTemp?: TObjectValue) => {
  let env = { ...process.env, ...(envTemp || {}) }
  // 此处假设gitFolder为当前文件夹下的git文件夹
  const gitFolder = path.join(__dirname, 'git')
  let gitPath = ''
  // windows下，git路径为gitFolder\cmd\git.exe
  if (process.platform === 'win32') {
    const win32GitSubfolder = getWin32GitSubfolder()
    gitPath = path.join(gitFolder, 'cmd', 'git.exe')
    env.GIT_EXEC_PATH = path.join(gitFolder, win32GitSubfolder, 'libexec', 'git-core')
    env.PATH = `${gitFolder}\\${win32GitSubfolder}\\bin;${gitFolder}\\${win32GitSubfolder}\\usr\\bin;${env.PATH ?? ''}`
  } else {
    // 其他平台下，git路径为gitFolder\bin\git
    gitPath = path.join(gitFolder, 'bin', 'git')
    env.GIT_CONFIG_SYSTEM = path.join(gitFolder, 'etc', 'gitconfig')
    env.GIT_EXEC_PATH = path.join(gitFolder, 'libexec', 'git-core')
    env.GIT_TEMPLATE_DIR = path.join(gitFolder, 'share', 'git-core', 'templates')
  }
  // 如果git路径不存在，使用系统git
  if (!fs.existsSync(gitPath)) {
    env = { ...envTemp }
    gitPath = 'git'
  }
  return { env, gitPath }
}

// 导出git命令函数
export const git = (args: string[], options: TObjectValue) => {
  if (!options) options = {}
  const { gitPath, env } = getGitPathAndGitEnv(options.env)
  options.env = env
  return spawn(gitPath, args, options)
}
```

## Feedback and Suggestions
Your valuable opinions are welcome through the following channels:
- Submit an Issue: [Issue](https://github.com/huangcs427/enjoy-git-release/issues)
- Email Contact: huangcs427@163.com
