---
title: Brew使用技巧
description: "Here is a sample of some basic Markdown syntax that can be used when writing Markdown content in Astro."
publishDate: 2025-11-12 00:00:00
# img: /assets/stock.jpg 可配置文章封面
# img_alt: Brew tutor
tags:
  - Brew
---

# Homebrew (brew) 常用指令指南

## 目录

- [基础操作](#基础操作)
- [查询和搜索](#查询和搜索)
- [维护和清理](#维护和清理)
- [Cask 应用管理](#cask-应用管理)
- [服务管理](#服务管理)
- [仓库管理](#仓库管理)
- [故障排查](#故障排查)
- [常用组合命令](#常用组合命令)

---

## 基础操作

### 安装软件包

```bash
# 安装软件包
brew install <formula>

# 安装指定版本
brew install <formula>@<version>

# 安装时显示详细信息
brew install --verbose <formula>

# 从源码编译安装
brew install --build-from-source <formula>
```

### 卸载软件包

```bash
# 卸载软件包
brew uninstall <formula>

# 强制卸载（忽略依赖）
brew uninstall --ignore-dependencies <formula>

# 卸载并删除所有版本
brew uninstall --force <formula>
```

### 更新和升级

```bash
# 更新 Homebrew 和所有配方
brew update

# 升级所有已安装的软件包
brew upgrade

# 升级指定软件包
brew upgrade <formula>

# 查看可升级的软件包
brew outdated

# 阻止自动更新
export HOMEBREW_NO_AUTO_UPDATE=1
```

### 重新安装

```bash
# 重新安装软件包
brew reinstall <formula>

# 重新安装并显示详细信息
brew reinstall --verbose <formula>
```

---

## 查询和搜索

### 搜索软件包

```bash
# 搜索软件包
brew search <text>

# 搜索已安装的软件包
brew list | grep <text>

# 使用正则表达式搜索
brew search /^git/
```

### 查看信息

```bash
# 查看软件包详细信息
brew info <formula>

# 查看软件包主页
brew home <formula>

# 列出所有已安装的软件包
brew list

# 列出软件包安装的文件
brew list <formula>

# 查看软件包依赖
brew deps <formula>

# 查看依赖树
brew deps --tree <formula>

# 查看哪些包依赖于指定软件包
brew uses <formula>

# 查看没有被其他包依赖的包
brew leaves
```

---

## 维护和清理

### 清理缓存

```bash
# 清理所有旧版本
brew cleanup

# 查看将要清理的内容（不执行）
brew cleanup -n

# 清理指定软件包的旧版本
brew cleanup <formula>

# 清理所有缓存（包括下载）
brew cleanup -s

# 自动清理（超过30天的）
brew cleanup --prune=30
```

### 系统诊断

```bash
# 检查系统问题
brew doctor

# 检查配置
brew config

# 删除不再需要的依赖
brew autoremove
```

### 版本控制

```bash
# 锁定软件包版本（防止升级）
brew pin <formula>

# 解锁软件包版本
brew unpin <formula>

# 查看已锁定的软件包
brew list --pinned
```

---

## Cask 应用管理

### 安装 GUI 应用

```bash
# 安装 macOS 应用
brew install --cask <app>

# 强制安装（跳过 SHA 验证）
brew install --cask --force <app>

# 安装特定版本
brew install --cask <app>@<version>
```

### 管理 Cask 应用

```bash
# 列出已安装的应用
brew list --cask

# 卸载应用
brew uninstall --cask <app>

# 重新安装应用
brew reinstall --cask <app>

# 升级所有 cask 应用
brew upgrade --cask

# 升级指定 cask 应用
brew upgrade --cask <app>

# 查看 cask 应用信息
brew info --cask <app>

# 搜索 cask 应用
brew search --cask <text>
```

### Cask 清理

```bash
# 清理下载的安装包
brew cleanup --cask

# 查看过期的 cask
brew outdated --cask
```

---

## 服务管理

### 基本服务操作

```bash
# 列出所有服务
brew services list

# 启动服务
brew services start <service>

# 停止服务
brew services stop <service>

# 重启服务
brew services restart <service>

# 运行服务（不设置自启动）
brew services run <service>

# 清理未使用的服务
brew services cleanup
```

---

## 仓库管理

### Tap 管理

```bash
# 添加第三方仓库
brew tap <user/repo>

# 列出已添加的仓库
brew tap

# 移除仓库
brew untap <user/repo>

# 更新指定仓库
brew tap --repair
```

### 常用第三方仓库

```bash
# Homebrew Cask Versions（不同版本的应用）
brew tap homebrew/cask-versions

# Homebrew Cask Drivers（驱动程序）
brew tap homebrew/cask-drivers

# Homebrew Cask Fonts（字体）
brew tap homebrew/cask-fonts
```

---

## 故障排查

### 诊断命令

```bash
# 全面系统检查
brew doctor

# 查看配置信息
brew config

# 查看环境变量
brew --env

# 查看缓存位置
brew --cache

# 查看安装路径
brew --prefix

# 查看仓库路径
brew --repository
```

### 修复问题

```bash
# 修复链接问题
brew link <formula>

# 强制重新链接
brew link --overwrite <formula>

# 取消链接
brew unlink <formula>

# 修复所有损坏的链接
brew cleanup && brew link --overwrite --dry-run
```

---

## 常用组合命令

### 完整更新流程

```bash
# 更新 Homebrew
brew update

# 升级所有软件包
brew upgrade

# 升级所有 cask 应用
brew upgrade --cask

# 清理旧版本
brew cleanup

# 检查系统
brew doctor
```

### 查找并安装

```bash
# 搜索并查看信息
brew search <package> && brew info <package>

# 安装并立即使用
brew install <package> && brew link <package>
```

### 完全卸载

```bash
# 卸载软件包及其依赖
brew uninstall --ignore-dependencies <formula>
brew autoremove
brew cleanup
```

### 备份已安装列表

```bash
# 导出已安装软件包列表
brew list --formula > ~/brew-formula.txt
brew list --cask > ~/brew-cask.txt

# 从列表恢复安装
xargs brew install < ~/brew-formula.txt
xargs brew install --cask < ~/brew-cask.txt
```

---

## 环境变量

### 常用环境变量

```bash
# 禁用自动更新
export HOMEBREW_NO_AUTO_UPDATE=1

# 设置镜像源（中国大陆）
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.aliyun.com/homebrew/homebrew-bottles

# 设置 brew 安装路径
export PATH="/opt/homebrew/bin:$PATH"  # Apple Silicon
export PATH="/usr/local/bin:$PATH"     # Intel Mac

# 显示更详细的输出
export HOMEBREW_VERBOSE=1

# 使用代理
export ALL_PROXY=socks5://127.0.0.1:1080
```

---

## 实用技巧

### 1. 快速升级所有内容

```bash
brew update && brew upgrade && brew upgrade --cask && brew cleanup
```

### 2. 查看大文件包

```bash
du -sch $(brew --cache)/*
```

### 3. 查看依赖关系图

```bash
brew deps --tree --installed
```

### 4. 批量操作

```bash
# 升级所有过期的包
brew outdated | xargs brew upgrade

# 清理所有包的旧版本
brew list --formula | xargs brew cleanup
```

### 5. 查看最近安装的包

```bash
ls -lt /usr/local/Cellar | head -20
```

---

## 注意事项

1. **定期维护**：建议每周运行一次 `brew update` 和 `brew cleanup`
2. **谨慎升级**：升级前先用 `brew outdated` 查看，重要软件可以用 `brew pin` 锁定版本
3. **备份重要配置**：升级前备份重要软件的配置文件
4. **使用 Cask**：GUI 应用优先使用 `brew install --cask`，便于统一管理
5. **查看日志**：遇到问题时，查看 `~/Library/Logs/Homebrew/` 下的日志文件

---

## 更多资源

- [Homebrew 官方文档](https://docs.brew.sh)
- [Homebrew Formulae](https://formulae.brew.sh)
- [Homebrew GitHub](https://github.com/Homebrew/brew)
- [Homebrew 中文镜像配置](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

---

```

```
