# 更新日志（TB710FU OriginOS 7 全能修复）

## v2.3（内置 LSPosed v3.3 + 原生杜比全景声 v1.1）- 2026-11-24
- **新增原生杜比全景声（Dolby Atmos v1.1）**：自动安装极简原生「杜比全景声」控制 App（含控制中心快捷磁贴），直接驱动本机联想原装 `libswdap.so` 与 `vendor.dolby.hardware.dms@2.0-service`，完整加载 69 项本机四扬声器原厂调音参数；支持动态/电影/音乐/人声/游戏 5 档模式、智能均衡器、环绕虚拟器、音量平衡与对话增强；支持扬声器 ↔ 蓝牙 ↔ USB 切换自动重绑，不改动 `audio_policy_configuration.xml`（零干扰 LHDC V5）。

## v2.2（内置 LSPosed v3.3）- 2026-11-24
- **新增在线一键更新与二合一打包**：接入 SukiSU/KSU 在线更新通道（国内直连加速），刷入/更新 KSU 模块时自动同步覆盖升级内置的 `TB710FU_OOS7_AllInOne_v3.3.apk`（同签名免重勾作用域）。
- **修复待机电量异常（核心突破）**：修复无大幅运动传感器（`mMotionSensor == null`）导致系统整晚无法进入 Deep Doze（深度打盹 = 0）的问题，解锁原生深度休眠；手写笔监听改用 `inotifyd` 零轮询并默认关闭笔息屏供电（`PEN_WAKEUP=0`）；`mtp-guard.sh` 未插线时降频至 30s。
- **解锁 OriginOS 7 高级材质 / 视觉质感**：补齐 `vivo_features.txt` 中 `vivo.software.glasseffectos7`（`level=0` 旗舰档）与 `vivo.software.liquidfusion`，恢复「设置 → 桌面、锁屏与壁纸 → 视觉质感 / 高级材质」完整选项。
- **修复通信共享下后台偷跑流量自动云同步**：限制 `com.bbk.cloud` 与 `com.vivo.gallery` 后台走计费移动流量，并拦截相册移动流量灰度绕过（Wi-Fi 下云同步与前台手动操作不受影响）。

## v2.1（配合 LSPosed v3.2）- 2026-11-23
- **修复蓝牙 LHDC V5 掉扬声器 + AAC 静音**：补齐 OriginOS 7 `audioserver` 实际加载的 `/vendor/etc/audio_policy_configuration.xml` 软解路由挂载。
- **修复联想手写笔在 120/144Hz 失灵与偶发断触**：放开真 120Hz（移除 `120 -> 144` 强制劫持，保留 144Hz 档位满血 144Hz）；连笔自动切真 120Hz（0ms 切换）+ Tab Pen Pro 自动开启 8192 级压感，断笔自动恢复 144Hz；对齐陀螺仪 `400Hz` 上限。
- **修复亮屏 5W / 息屏 40W 快充温控**：清零 Vivo 禁用高通 PD/PPS 属性，并在 `/vendor` overlay 上层挂载放宽后的 `thermal-engine_battery_0.conf`。
- **修复「应用信息 → 解除设置限制」点击无响应**：修复 OriginOS 7 `Settings.apk` 上游空锚点 Bug。
- **修复「开发者选项」3 处条件性闪退**：为 LHDC 蓝牙编解码、通信共享虚拟卡 UE Mode、第三方 `ro.oem_unlock_supported=1` 增加防闪退兜底。
