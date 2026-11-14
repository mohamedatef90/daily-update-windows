# Daily Update - Windows Edition

A Windows desktop app (Electron) that detects installed apps, CLIs, runtimes, and git repositories, checks them for updates, and lets you choose what to update or install. It is the Windows port of the Daily Update macOS app, and this repository also contains the macOS Swift sources.

## Features

- **Known apps and tools:** bundled detectors for Cursor, Codex, Claude, ChatGPT, Warp, Ollama, Visual Studio Code, Claude Code, GitHub CLI, Node.js, npm, pnpm, Yarn, Bun, Python, Go, Flutter, Git, and more (`src/detectors.json`).
- **Git repository discovery:** recursively scans a root folder for git repositories (default depth 4, configurable from 1 to 10; skips hidden folders and directories such as `node_modules`, `dist`, and `vendor`; does not scan inside a repository once found).
- **Update checks:** `git fetch` and a commits-behind count for repositories; `winget` for apps and tools that have a known winget package ID.
- **Selective actions:** check items, then use "Update Selected" (updates, or installs items that are not installed). A confirmation dialog appears before anything runs.
- **Discover Apps view:** lists known apps that are not installed and can be installed through winget.
- **Dashboard, category views (Apps, CLIs, Repos, Runtime/Library), Updates Available, and History.**
- **Settings persistence:** the root folder, application folders, scan depth, and the last scan results are saved locally.

## Requirements

- Windows 10 or 11
- Node.js and npm
- Git
- winget (used for apps and tools that have a winget package ID)

## Getting started

```bash
npm install
npm start
```

On first launch a setup dialog asks for the root folder, application folders, and maximum scan depth. After setup, use the toolbar: Add Item, Refresh installed apps, Check Updates (Ctrl+R), and Update/Install Selected (Ctrl+U). "Select All Updates" and "Deselect All" act on the current view.

### Scripts

| Command | Purpose |
| --- | --- |
| `npm start` | Run the app with Electron |
| `npm test` | Run `test/scanner.test.js` (repository scanner tests) |
| `npm run build` | Unpacked Windows build (`electron-builder --win --dir`) into `dist/` |
| `npm run package` | Windows NSIS installer (`electron-builder --win`) into `dist/` |

## How updates work

- **Git repositories:** `git fetch`, then a count of commits behind the upstream branch; updating runs `git pull --ff-only`.
- **Apps and tools with a winget ID:** `winget upgrade` to update and `winget install` for items that are not installed.

## Settings

Saved to `.daily-update-windows\settings.json` in your user profile folder (`src/settings.js`).

## Project structure

```
src/
  main.js          Electron main process and IPC handlers
  preload.js       IPC bridge
  renderer.js      UI logic
  index.html       Main window and setup dialog
  styles.css       Styles
  scanner.js       Git repository scanner
  appScanner.js    Installed-app scanning
  detectors.js     Detector loading and matching
  detectors.json   Bundled app, CLI, and runtime detectors
  updater.js       Git and winget update logic
  settings.js      Settings persistence
  pathUtils.js     Path helpers
test/scanner.test.js
Assets/            App icons
Sources/, Package.swift   macOS (SwiftUI) version of the app
```

`README-WINDOWS.md` and `PORT-SUMMARY.md` hold earlier notes on the Windows port.

## Troubleshooting

- **Git operations fail:** install Git and make sure `git --version` works in a terminal.
- **winget updates or installs fail:** install winget (App Installer from the Microsoft Store on Windows 10) and make sure `winget --version` works.
- **No repositories found:** check that the root folder contains git repositories, raise the maximum scan depth, and make sure the repositories are not under a skipped directory.

## License

MIT, as declared in `package.json`. The repository has no separate LICENSE file.
