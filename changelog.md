# 更新日志（TB710FU OriginOS 7 全能修复）

## v2.3.1（重要热修，强烈建议升级）- 2026-11-24
- **修复在线更新后子脚本丢失可执行权限（P0）**：恢复 `sm8650q_sched.sh`（六核专属调度/cpuset）、`boot/lid-daemon.sh` & `lid-wake.sh`（磁吸皮套亮息屏）、`boot/mtp-guard.sh`（MTP 守护）与 `patch_ld.sh` 正常执行，并在 ZIP 元数据、`customize.sh` 与调用端做三层权限加固。
- **修复重启锁屏 FBE 解锁前竞态（P0）**：将杜比全景声自启、`netpolicy` 通信共享防偷跑云同步、反诈 CE 配置与 HttpDNS 缓存清理统一改为等待解锁后自动触发，确保每次重启解锁 100% 自动生效。

## v2.3（内置 LSPosed v3.3 + 原生杜比全景声 v1.1）- 2026-11-24
- **新增原生杜比全景声（Dolby Atmos v1.1）**：自动安装极简原生「杜比全景声」控制 App（含控制中心快捷磁贴），直接驱动本机联想原装 `libswdap.so` 与 `vendor.dolby.hardware.dms@2.0-service`，完整加载 69 项本机四扬声器原厂调音参数；支持动态/电影/音乐/人声/游戏 5 档模式、智能均衡器、环绕虚拟器、音量平衡与对话增强；支持扬声器 ↔ 蓝牙 ↔ USB 切换自动重绑，不改动 `audio_policy_configuration.xml`（零干扰 LHDC V5）。

## v2.2（内置 LSPosed v3.3）- 2026-11-24
- **新增在线一键更新与二合一打包**：接入 SukiSU/KSU 在线更新通道（国内直连加速），刷入/更新 KSU 模块时自动同步覆盖升级内置的 `TB710FU_OOS7_AllInOne_v3.3.apk`。
- **修复待机电量异常**：修复 `mMotionSensor == null` 导致整晚 Deep Doze = 0 的问题；手写笔改 `inotifyd` 零轮询 + 默认关笔息屏供电；`mtp-guard.sh` 未插线降频至 30s。
- **解锁 OriginOS 7 高级材质 / 视觉质感**：补齐 `vivo.software.glasseffectos7`（`level=0`）与 `vivo.software.liquidfusion`。
- **修复通信共享下后台偷跑流量自动云同步**：限制 `com.bbk.cloud` 与 `com.vivo.gallery` 后台计费流量，拦截相册灰度绕过。

## v2.1（配合 LSPosed v3.2）- 2026-11-23
- 修复蓝牙 LHDC V5 掉扬声器 + AAC 静音、手写笔真 120Hz + Pro 笔 8192 级压感、亮屏 40W 快充温控、应用信息解除受限设置、开发者选项 3 处条件性闪退。
