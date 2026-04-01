# Windows 安装设置

[返回 Day 0](README_ZH.md)

---

**Node.js**
- 访问 [nodejs.org](https://nodejs.org)
- 点击 **"Download Node.js (LTS)"** 按钮 — 这会下载 `.msi` 安装程序
- 运行 `.msi` 文件，在向导中点击 **Next**
- 接受默认选项，点击 **Install**，等待完成

**验证 Node.js**
- 打开一个**新**终端（PowerShell 或 Windows Terminal）并运行：
  ```powershell
  node --version
  npm --version
  ```

**Claude Code**
- ```powershell
  npm install -g @anthropic-ai/claude-code
  ```
- 如果遇到权限错误，以**管理员身份**运行终端（右键 > 以管理员身份运行）

**验证**
- ```powershell
  claude --version
  ```

---

现在返回 [README_ZH.md](README_ZH.md) 进行认证设置。