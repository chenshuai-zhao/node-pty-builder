# node-pty-builder

用 GitHub Actions 为「京智办」编译 node-pty 的 **Linux 原生模块**(Electron 38 / ABI 137 / N-API),
产出 `linux-x64` 和 `linux-arm64` 两套 `{ pty.node, spawn-helper }`。

> 这个仓库里**没有任何业务代码**,只有调用开源 node-pty + prebuildify 的编译脚本。
> 保留它,以后升级 node-pty 时改 `.github/workflows/build.yml` 里的版本号,重跑一次即可。

---

## 使用步骤（你只需点几下，不用本地编译）

### 1. 在 GitHub 建一个仓库
- 打开 https://github.com/new
- 仓库名随便（如 `node-pty-builder`），**public 或 private 都行**（用 Actions 即可）
- 不要勾选任何初始化文件，建空仓库

### 2. 把本目录推上去
在本目录（`~/Desktop/node-pty-builder/`）打开终端：
```bash
cd ~/Desktop/node-pty-builder
git init
git add .
git commit -m "build node-pty linux prebuilds"
git branch -M main
git remote add origin https://github.com/<你的用户名>/node-pty-builder.git
git push -u origin main
```
（推送时按提示登录 GitHub）

### 3. 等 Actions 自动编译
- 推送后 GitHub 会**自动开始编译**（也可在仓库的 **Actions** 标签页手动 **Run workflow**）
- 约 2-4 分钟，两个架构（x64 / arm64）并行编
- 绿色对勾 = 成功

### 4. 下载产物
- 进仓库 **Actions** → 点最新那次运行
- 页面底部 **Artifacts** 区有两个包：
  - `node-pty-linux-x64`
  - `node-pty-linux-arm64`
- 下载这两个 zip，解压

### 5. 把产物交给开发
解压后每个里面是：
```
linux-x64/pty.node
linux-x64/spawn-helper
linux-arm64/pty.node
linux-arm64/spawn-helper
```
把这两个目录给开发（或告诉路径），接入「京智办」打包流程。

---

## 锁定的版本（勿随意改）
- node-pty: **1.1.0**
- Electron: **38.7.0**（ABI 137）
- 编译方式: prebuildify --napi（N-API，跨 ABI 通用）

如果「京智办」将来升级了 Electron 或 node-pty，改 `build.yml` 顶部的 `env` 版本号，重跑即可。
