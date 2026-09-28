# 生活台 · 原生 Android App 工程（LifeDeskApp）

把原来的网页工作台打包成**真正的手机 App**。App 用 Android WebView 内嵌网页，
数据写入 App 自己的私有目录（`/data/data/com.lifedesk/...`），**关掉再打开不会丢失**——
这正是华为浏览器“添加到主屏幕”做不到的。完全离线、不上传任何数据。

## 工程结构
```
LifeDeskApp/
├── settings.gradle
├── build.gradle
├── gradle.properties
├── gradle/wrapper/gradle-wrapper.properties
└── app/
    ├── build.gradle
    ├── proguard-rules.pro
    └── src/main/
        ├── AndroidManifest.xml
        ├── java/com/lifedesk/MainActivity.java   # WebView 容器 + 沉浸式全屏
        ├── res/                                  # 布局/主题/图标
        └── assets/index.html                     # 工作台网页本体
```

## 怎么装到手机（不用装 Android Studio：GitHub 云编译，推荐）

你这边只做“注册账号 + 上传 + 点一下”，编译由 GitHub 免费服务器完成，约 2–4 分钟。

1. 打开 https://github.com ，注册一个**免费**账号（用邮箱即可）。
2. 右上角 **+ → New repository**，名字随便（如 `lifedesk`），选 **Public**，
   勾选 **Add a README file**，点 **Create repository**。
3. 进入新仓库 → 点 **Add file → Upload files** → 把本 `LifeDeskApp` 整个文件夹
   **拖进去**（会保留 `.github/workflows/build-apk.yml` 等子目录结构）→ 点 **Commit changes**。
4. 顶部切到 **Actions** 标签 → 看到 “Build LifeDesk APK” → 点 **Run workflow → Run**。
5. 等约 2–4 分钟，状态变绿 ✅ → 点进该任务 → 右侧 **Artifacts** 下载 `lifedesk-apk.zip`
   → 解压得到 `app-debug.apk`。
6. 把 `app-debug.apk` 传到手机（微信文件传输助手 / 数据线 / 网盘），在手机上点它安装。
   若提示“禁止安装未知应用”，按提示**允许本次安装**即可。

> 之后想更新工作台内容：改完 `assets/index.html` 重新上传并提交，再到 Actions 重跑一次即可。

## 备选：用电脑 Android Studio（最稳，但需要装软件）

1. **装 Android Studio**：官网 https://developer.android.com/studio 免费下载安装。
2. **手机开调试**：设置 → 关于手机 → 连续点“版本号”7 次开启开发者模式；
   返回 → 系统和更新 → 开发人员选项 → 打开 **USB 调试**。
3. **连电脑**：数据线连手机，手机通知选“传输文件(MTP)”，弹窗点“允许 USB 调试”。
4. **打开工程**：Android Studio → Open → 选中本 `LifeDeskApp` 文件夹 →
   等待 **Gradle 同步**（首次会自动下载 Gradle 8.2 和 Android SDK，按提示点 Install 即可）。
5. **运行**：顶部设备栏选你的手机 → 点绿色 ▶ **Run**。
   编译完自动装到手机并打开，桌面就有“生活台”图标。

## 想生成安装包 APK（备份或发给别人）
Android Studio 菜单：**Build → Build Bundle(s) / APK(s) → Build APK(s)**。
完成后打开输出文件夹，把 `app-debug.apk` 传到手机安装即可。用上面的 GitHub 云编译也会产出同样的 `app-debug.apk`。

## 数据说明
- 数据存在 App 私有目录，手机系统**不会**像浏览器那样自动清空；关掉再开、重启手机都在。
- **卸载 App 会删除数据**，建议重要内容记在别处或将来做导出备份。
- 在 App 内，“导出/导入备份”按钮（依赖浏览器下载/文件选择）在 WebView 里可能不工作；
  如需备份，可等后续版本补充。不影响日常录入与持久保存。
- 与原来的网页版（HTTPS / 本地文件）不是同一来源，数据不互通；旧网页里的数据需手动重新录入。
- 更新页面：改完 `life-workspace.html` 后，重新拷成 `app/src/main/assets/index.html` 再编译。
