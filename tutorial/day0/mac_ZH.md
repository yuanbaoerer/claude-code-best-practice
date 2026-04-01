# macOS 安装设置

[返回 Day 0](README_ZH.md)

---

**终端**
- 打开终端（按 `Cmd + Space`，输入 "Terminal"，按 Enter）

**Homebrew**
- 检查 Homebrew 是否已安装：
  ```bash
  brew --version
  ```
- 如果显示 "command not found"，先安装 Homebrew：
  ```bash
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```

**Claude Code**
- ```bash
  brew install --cask claude-code
  ```

**验证**
- ```bash
  claude --version
  ```

---

现在返回 [README_ZH.md](README_ZH.md) 进行认证设置。