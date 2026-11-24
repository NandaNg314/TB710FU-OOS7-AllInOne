# 更新日志（TB710FU OriginOS 7 全能修复）

## v2.4（蓝心小V硬件唤醒 + 通信共享长稳加固 + QQ横屏修复，内置 LSPosed v3.4）- 2026-11-24
- **新增蓝心小V（“小V小V”）ADSP LPI 硬件语音唤醒**：内置 `UserDefinedMi.udm`、`udk7_opaque.bin`、修补版 `liblsmclient.so`、4 条 `ListenSoundModel` SELinux 规则及 `XiaoVUdk7Hook`（合入 `TB710FU_OOS7_AllInOne.apk` `v3.4`）。实测走高通 ADSP 硬件低功耗通道（`powerConsumptionMw: 0`，零 AP 录音、零常驻唤醒锁），不影响待机续航。
  - **老版本升级提示**：覆盖更新重启后，请在 Vector / LSPosed 管理器中为 `TB710FU_OOS7_AllInOne` 勾选新增作用域 **`语音唤醒`（`com.vivo.voicewakeup`）** 后即可立即生效。
- **通信共享 4 项长稳加固**：`persist.radio.multisim.config=dsds` 持久化防重载丢失；移除 `vendor.vivorild` 的 `oneshot` 实现崩溃自动拉起；开机双检 `vcmsnet00`/`vcmsnet10`；将 `com.vivo.connbase`、`com.vivo.connbase.connectcenter`、`com.vivo.vdfs`、`com.vivo.vmdri` 加入 `deviceidle` 白名单防后台冻结。
- **修复 QQ 群文件等子界面强制竖屏**：开机自动对 `com.tencent.mobileqq` 开启系统原生 `UNIVERSAL_RESIZABLE_BY_DEFAULT`（`357141415`），保持平板横屏全屏且不影响其他应用。
- **脚本与细节收口**：合并重复 `anti_fraud_file.xml` 配置块；修复 `usb-mtp-adb.sh` / `mtp-guard.sh` 时间戳顺序与算术笔误；`netpolicy` 补充 `com.android.notes`；自动迁移旧版 `tb710_pen.conf` 的 `PEN_WAKEUP=1 -> 0`。

## v2.3.2（修复杜比全景声真生效）- 2026-11-24
- **修复杜比全景声在音乐/视频通道未生效的问题**：在 `/vendor/etc/audio/sku_pineapple/audio_effects.xml` 的 `<stream type="music">` 后处理链加入 `<apply effect="dap"/>`，使 B 站、音乐 App 的外放（`deep_buffer`）与蓝牙耳机音频全部实时进入原生 `libswdap.so` 处理引擎，切换模式与均衡器 2ms 内实时生效。

## v2.3.1（重要热修）- 2026-11-24
- **修复在线更新后子脚本丢失可执行权限（P0）**：恢复 `sm8650q_sched.sh`、磁吸皮套守护、MTP 守护与 `patch_ld.sh` 正常执行，并在 ZIP 元数据、`customize.sh` 与调用端做三层 `0755` 加固。
- **修复重启锁屏 FBE 解锁前竞态（P0）**：将杜比全景声自启、`netpolicy` 通信共享防偷跑云同步、反诈 CE 配置与 HttpDNS 缓存清理统一改为等待用户解锁后自动触发。

## v2.3（内置 LSPosed v3.3 + 原生杜比全景声 v1.1）- 2026-11-24
- 新增原生杜比全景声控制 App（含控制中心快捷磁贴），直接驱动本机联想原装 `libswdap.so` 与 69 项四扬声器调音参数，零干扰 LHDC V5。

## v2.2（内置 LSPosed v3.3）- 2026-11-24
- 新增 SukiSU/KSU 国内直连在线更新与一体打包、修复 Deep Doze 深度打盹 = 0 待机耗电、解锁 OriginOS 7 高级材质/视觉质感、修复通信共享下后台偷跑云同步。

## v2.1（配合 LSPosed v3.2）- 2026-11-23
- 修复蓝牙 LHDC V5 掉扬声器 + AAC 静音、手写笔真 120Hz + Pro 笔 8192 级压感、亮屏 40W 快充温控、应用信息解除受限设置、开发者选项 3 处条件性闪退。
