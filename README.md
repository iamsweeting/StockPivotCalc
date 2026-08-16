# Android APK 云编译仓库（GitHub Actions）

本仓库包含两个 Flet 股票应用，推送（或手动触发）后由 **GitHub 云端服务器**自动编译出两个可直接安装到 Android 手机的 APK，无需本地安装任何 Android 工具链。

## 包含的两个应用

| 应用 | 源文件 | 图标 | 包名（Application ID） | APK 文件名 |
|------|--------|------|------------------------|------------|
| 股票枢轴点（单股版） | `StockPivot/main.py`（源自 stockv1.5.py） | `StockPivot/assets/icon.png`（源自 stockV1.5.png） | `com.pivottool.stockpivot` | `StockPivotCalc.apk` |
| 股票枢轴点（批量版） | `BatchStock/main.py`（源自 BatchStockV2.py） | `BatchStock/assets/icon.png`（源自 BatchStockV2.png） | `com.pivottool.batchstock` | `BatchStock.apk` |

> 两个包名不同，**可同时安装在同一部手机上**，图标各自使用对应的 PNG 文件。

## 使用方法（三步）

### 1. 新建 GitHub 仓库并推送

- 打开 [github.com](https://github.com)，点 **New repository**，仓库名随意（如 `flet-stock-apks`），选 Public 或 Private 均可。
- 把本目录内容推上去（任选一种）：

  **方式 A：网页上传（无需装 Git）**
  在仓库页面点 **Add file → Upload files**，把本目录里的 `StockPivot`、`BatchStock`、`.github` 三个文件夹连同里面的文件全部拖进去（注意保留 `.github/workflows/build-apks.yml` 的目录结构），提交。

  **方式 B：Git 命令行**
  ```bash
  git init
  git add .
  git commit -m "init"
  git remote add origin https://github.com/<你的用户名>/<仓库名>.git
  git push -u origin main
  ```

### 2. 等待云端编译

推送后自动触发，或在仓库页点 **Actions** 选项卡 → 左侧 **Build Android APKs** → **Run workflow** 手动触发。
首次编译约 15–30 分钟（云端要下载 Flutter、Android SDK、Python 依赖），之后走缓存会快很多。

### 3. 下载 APK 并安装到手机

- 编译完成后，回到 **Actions** 页面，点开最新一次运行，在底部 **Artifacts** 处下载 `android-apks`（一个 zip，内含两个 APK）。
- 解压后把 `StockPivotCalc.apk`、`BatchStock.apk` 传到手机（微信/网盘/数据线均可），点击安装。
  - 手机需允许"安装未知来源应用"。
  - 两个 APK 包名不同，可同时安装、同时使用。

## 常见问题

- **编译失败**：点开失败的步骤看日志；常见原因是 GitHub 网络波动导致依赖下载失败，直接 **Re-run jobs** 重跑即可。
- **想改图标/代码**：替换对应 `assets/icon.png` 或 `main.py` 后重新推送即可，云端会重新编译。
- **想支持 32 位旧手机**：把工作流里两处 `--arch arm64-v8a` 改成 `--arch arm64-v8a armeabi-v7a`（APK 会变大）。
- **flet 版本升级**：若 flet 发布新版本，可把工作流中的 `flet==0.86.5` 和 `flutter-version: "3.44.8"` 相应更新。
- **不想用 GitHub**：同样思路可用于 Gitee 码云（Gitee Go）或任意 Linux 云服务器（安装 Python/Flutter/Android SDK 后执行同样的 `flet build apk` 命令）。

## 目录结构

```
.
├── .github/workflows/build-apks.yml   # GitHub Actions 云编译配置
├── StockPivot/                        # 单股版应用
│   ├── main.py                        # 入口程序（stockv1.5.py）
│   ├── requirements.txt               # Python 依赖
│   └── assets/icon.png                # 应用图标（stockV1.5.png）
└── BatchStock/                        # 批量版应用
    ├── main.py                        # 入口程序（BatchStockV2.py）
    ├── requirements.txt               # Python 依赖
    └── assets/icon.png                # 应用图标（BatchStockV2.png）
```
