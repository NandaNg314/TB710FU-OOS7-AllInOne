# 更新日志（TB710FU OriginOS 7 全能修复）

## v2.5.4 (2026-11-25)
- **修复 `service.sh` 开机时序 Bug（推荐所有用户更新）**：将原子隐私系统（`XSpace` 建 `virtual_system` + 恢复设置入口）、多应用强制横屏（`am compat`）与默认时区兜底移至 `sys.boot_completed=1` + 10s 后一次性执行，彻底解决部分升级用户开机过早调用 `pm/am/cmd` 导致原子隐私系统入口未自动恢复的问题（零常驻、零轮询、不影响 Deep Doze）
- 完整包含 `v2.5.3` 全部功能（时区列表空白修复、原子隐私系统、物理微距副摄直连、SukiSU WebUI 控制台、HDR 亮度同步、息屏防卡顿）

## v2.5.3 (2026-11-25)
- **彻底修复时区列表空白与双清默认格林尼治（GMT+0）**：在 `vivo_features.txt` 补齐 `vivo.software.timezonecorrection$vivo-support` 恢复 `tz_moniter` 系统服务与 247 城市手动时区列表；`service.sh` 新增条件兜底，仅在未设置或默认 GMT/UTC 时自动设为 `Asia/Shanghai`
- 完整包含 `v2.5.2` 全部功能（原子隐私系统零轮询修复、物理微距副摄直连、联想相机拍照修复、SukiSU WebUI 控制台、HDR 亮度同步、息屏防卡顿）

## v2.5.2 (2026-11-25)
- **彻底修复原子隐私系统（`XSpace`）**：恢复 `/data/vivo-common` 原厂 `0777` 权限 + 一次性幂等初始化 `/data/vivo-common/virtual_system`（`0770` + `vivo_data_file`）与首次设置入口恢复，**零后台轮询、不破坏 Deep Doze**；真机冷重启实测隐私空间（user 666）创建、切换与持久化全部正常
- **新增作用域醒目提醒**：在 SukiSU Nova 模块说明与刷入完成输出中提醒用户重启后到 Vector / LSPosed 重新勾选推荐作用域
- 完整包含 `v2.5.1` 全部功能（真物理微距副摄直连、联想相机拍照修复、SukiSU WebUI 控制台、HDR 亮度同步、息屏防卡顿）

## v2.5.1 (2026-11-25)
- **真正点亮后置物理微距副摄（`Device 2`）**：新增 `sepolicy.rule` 放行 `cameraserver` 读取 `camera2_extensions_prop`（解除 `expose Aux 0` 拦截），配合 LSP **v3.7.1 (38)** 将联想相机微距直连物理副摄 `Device 2`（实测 `CamX cameraId: 2` 正常取景与拍照）
- 完整包含 `v2.5` 全部功能（SukiSU WebUI 控制台、自选强制横屏、可调 ZRAM/GPU、HDR 亮度同步、息屏防卡顿）

## v2.5 (2026-11-25)
- **新增 SukiSU WebUI 控制台**：系统全项体检、自选应用强制横屏管理器（`357141415` 持久化）、ZRAM 可调（关闭/4G/6G/8G/12G 与 lz4/zstd）、Adreno 750 GPU 灵敏度即时调节、HDR/人脸补光/双击亮屏快捷开关
- **修复联想相机拍照与微距闪退**（LSP v3.7）：绕过 `libmorpho_ImageRefiner.so` 拍照 `SIGILL`，从 `CameraId` 源头与 `CameraManager` 修复微距 `-1` 越界与故障 13
- **修复 HDR / UltraHDR 亮度同步**（LSP v3.7）：同步真实背光 nit 值到 `SurfaceFlinger`，移除虚假 `vivo.hardware.display.xdr` 声明
- **息屏卡顿与细节打磨**：屏蔽 Q4 独显探测（`persist.vivo.displayic.enable=false`）、不再覆写人脸解锁补光设置、补全 `logpersistd_logging_prop` SELinux 规则、开启 MGLRU `min_ttl_ms=1000`
- 内置 LSPosed 模块升级至 **v3.7 (37)**，原生杜比全景声保持 **v1.2 (12)**

## v2.4.3 (2026-11-25)
- **录音与语音输入音量修复**：`mixer_paths_pineapple_mtp.xml` 将内置麦 `speaker-mic` 数字前级 `TX_DEC0/2 Volume` 从 `84`（0 dB）提升至 `96`（+12 dB，响度提升 4 倍），保持 `ADC1/2=12` 不变，零影响小V 硬件唤醒与通话 AEC。
- **GPU 频率锁定修复与调频优化**：`sm8650q_sched.sh` 将 `default_pwrlevel` 恢复为 `11`（231MHz，彻底解除空闲锁 366MHz），同时启用 `devfreq/mod_percent=120`（+20% 动态升频灵敏度），兼顾空闲省电、高级材质流畅与游戏 903MHz 满血释放。

## v2.4.2 (2026-11-24)
- **三指手势修复**：补齐 `vivo.software.globalgesture`，恢复三指下滑截屏、三指上滑分屏与三指区域截屏。
- **冷启动流畅度优化**：新增 6 核 `targetconfig_6core.xml` 修复高通 `perf2-hal` 拓扑；内置 LSPosed 模块升至 **v3.6**（纯 Java 拦截缺失 `IVPerf` 阻塞，无需新勾作用域）；`sm8650q_sched.sh` 优化 `input_boost_ms=120` 与 `top-app uclamp.min=15`。
- **高级材质 GPU 起步优化**：Adreno 750 起步唤醒档位 `default_pwrlevel=9`（366MHz），缓解退出控制中心实时模糊掉帧，空闲仍正常回落 231MHz。
- **原生杜比全景声 v1.2**：按场景模式独立保存并对齐原厂调音、消除播歌重连抖动、彻底隐藏左上角状态栏 `DO` 图标与常驻通知。

## v2.4.1（修复设置搜索栏悬浮键盘卡死与重影白屏，内置 LSPosed v3.5）- 2026-11-24
- **修复系统设置搜索栏在悬浮键盘 / 实体键盘下无法进入搜索态、透底重影或左栏白屏**：在 `TB710FU_OOS7_AllInOne.apk`（`v3.5`）新增 `SettingsSearchFixHook`，绕过 OriginUI `VSearchView`（`wa.z.g3()`）对底部 IME `WindowInsetsAnimation` 的死等逻辑，点击搜索框即可瞬间切入搜索态并显示「搜索建议」、右侧 `×` 取消按钮与弹起悬浮键盘，退出干净无白屏（悬浮键盘 / 常规软键盘 / 磁吸实体键盘全兼容）。

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
