# 更新日志（TB710FU OriginOS 7 全能修复）

## v2.8.1 (versionCode: 281 / AioFix v4.1 / Dolby v1.3) - 2026-11-26
- **根治游戏态「游戏魔盒重播进入动画」联动「通信共享」断连**：定位并拦截 `system_server` 内 `RMAms.killProcess` / `stopPackage` / `VivoAmsImpl.forceStopPackageByRms` 在游戏中因内存压力误杀 `com.vivo.gamecube:assistantui`（重拉起冷启动重播进游戏横幅并在 3 秒后触发清后台），并为 `com.vivo.connbase*` / `vdfs` / `handoff` / `vmdri` / `linkservice` 全链路加上防强杀与防冻结护盾，彻底解决游戏中通信共享掉线问题。
- **修复截图动画明显掉帧（144Hz 瞬时脉冲 + 原生 AOT 编译）**：针对移植包 `com.vivo.smartshot` / `upslide` / `systemui` / `card` 默认处于 `[status=verify]` 纯解释执行及日常省电档 GPU 卡在 `231MHz` 的问题，新增刷入/开机单次 `speed` 原生编译与截屏绑定瞬间 `1.8s` GPU/DDR/Gold-B 瞬时提频脉冲（不切温控档、不重启 `thermal-engine`，动画结束自动归位省电档），实测截图掉帧率压降至 `6.59%`、GPU P95 稳定 `7ms`。
- **自愈恢复「智能侧边栏」与「原子组件商店」**：移除 `v2.8` 误禁用列表中的 `com.vivo.card`（`SuperCard.apk` 智能侧边栏插件本体）与 `com.vivo.cardstore`，刷入与开机自动恢复启用并自愈因预测性返回互斥遗留的 `upslide_open_quick_app_small_window=close` 死锁状态。
- **大型游戏（如《金铲铲之战》）防闪退与 `8G/12G/16G` 全运存自适应调度**：
  - 游戏内呼出小窗/通知失焦新增 **`15 秒` 性能降级防抖**，彻底消除切小窗瞬间 `0ms` 跌落日常频点与重启 `thermal-engine` 造成的帧率塌陷；修复高温退档误清零 `cooling_device` 引发的瞬态压降；
  - 拦截 Vivo `SPS/PEM` 低内存自杀机制误杀前台/小窗及近期活跃游戏，自动剔除 `forbidBgStartAppsName.txt` 游戏黑名单；
  - 移除阻塞 `kswapd` 回收的 `lru_gen/min_ttl_ms=1000`、开机桌面期 `swapoff` 内存冲击及全局强锁游戏 `144Hz` 属性；按物理运存（`8GB` vs `12GB/16GB`）动态匹配 ZRAM、`swappiness` 与 `LMKD` 阈值。
- **根治 HDR / UltraHDR 小窗失效与 5 分钟衰减**：解除 `display_id_hdr.xml` 的 `timeMaxSecs=300`（5 分钟硬限时）与省电拦截，将触发面积阈值下调至 `0.5%` 支持自由小窗 HDR，并在 `HdrBrightnessFixHook` 实现内置主屏与虚拟屏严格隔离及小窗 XDR 免旁路。
- **恢复系统设置「时间光影」与「重绘图标」原生开关**：精准区分官方内置主题与第三方下载 `.itz` 主题包，在官方主题下完整恢复 `设置 → 桌面、锁屏与壁纸 → 图标` 中的「时间光影」与「重绘图标」原生入口，同时保持第三方 `.itz` 图标包零冲突共存。
- **SukiSU WebUI 精简与「精细 150 级 / 联想原厂 15 级」音量模式切换**：移除与系统设置重复的 4 个冗余开关；新增「精细 150 级」与「联想原厂 15 级」音量步进无损换算切换、运存规格自适应识别与近期游戏异常退出原因诊断面板。

## v2.8 (versionCode: 280 / AioFix v4.0 / Dolby v1.3) - 2026-11-26
- **根治杜比全景声后台被杀与冻结断链**：新增 `system_server` 防强杀与防冻结保护（拦截 Vivo Cleaner / RMS / `FreezerHandle`）及音频起流自愈保活，Session-0 `dap` 全局音效全天候在线；首次升级自动向控制中心追加「杜比全景声」快捷磁贴。
- **消除音乐 App 硬件直通（Offload）绕过杜比无音效**：关闭 `compressed_offload` 旁路，QQ音乐 / 酷狗 / i音乐 / 网易云等统一走 `deep_buffer` 100% 经 `libswdap.so` 杜比渲染。
- **原生杜比全景声升级至 v1.3（V2+V3 签名）**：默认完整保留联想原厂 `dax-default.xml` 针对四扬声器与耳机分别精调的 20 段频响及人声/IEQ 参数，新增「恢复原厂调音」按钮。
- **修复四扬声器倒置横屏（180° 翻转）左右声道颠倒**：打通联想 HAL 私有参数 `screen_rotation=0|180` → `pal_set_degree()` → `aw_ar_dsp_write_spin()` 实时翻转四扬声器声道矩阵。
- **基于联想原厂 40KB `sku_pineapple` 重建统一 Audio Policy**：彻底移除不存在的虚拟听筒 `Earpiece`（修复微信语音 / VoIP / 通信共享未插耳机时默认无声），恢复系统 `Spatializer` 空间音频、蓝牙 LE Audio、Type-C DP/HDMI 音频、384kHz Hi-Res `direct_pcm`、通话消回声 `Echo Reference` 与 24-bit 录音，完整保留软件 `LHDC` 与蓝牙通话 `BT SCO`。
- **6 核 SM8650Q 硬件中断与 Wi-Fi 7 RPS 精准分流**：将 144Hz 显示/触控/GPU 中断绑定 `Gold-A (cpu1-2)`，UFS/蓝牙串口/USB3/IPCC 与 `wlan0` RPS 分流至 `Gold-B (cpu3-4)`，解放单小核 `cpu0`；UFS 预读收敛至 `256KB`。
- **修复游戏退回日常模式调度残留 Bug**：`tb710_perf_apply.sh` 加入原子互斥锁与双向安全写入器，100% 对称还原 WALT 迁移阈值、`SBT`、`DDR memlat`、`L3 boost` 与 GPU `wake_timeout=60`。
- **消灭后台重启风暴与 SELinux 拒绝**：停用不支持 `soc_id=696` 的 `vendor.qlm-service`（消除 5s 重启死循环与 72.9% AVC 刷屏）、`qti-testscripts`、`console` 与 `mobile_log_d`，实现全机 `dmesg` 零 SELinux AVC 拒绝。
- **桌面「时间光影」与 `.itz` 主题完美共存 + 细节加固**：切换开放/已下载图标不再丢失时间光影开关记忆；HDR 亮度同步严格隔离内置主屏（Type-C 外接屏互不干扰，关开关秒级恢复 SDR）；直驱供电实时同步 `charging_enable` 节点；双击亮屏改为进程内直写（零 `inotifyd`）；`FaceEngineService` 增加调用方 UID 安全鉴权；禁用红外/NFC/钱包等手机幽灵包与死磁贴，多窗上限升至 6 个；升级 `v2.8` WebUI。

## v2.7 (AioFix v3.9) - 2026-11-26
- **修复蓝牙耳机通话（BT SCO）无声**：补全 `audio_policy_configuration_lhdc_sw.xml` 4 个 BT SCO 端口 `<profile>`，并精准 8 字节修补 `audio.primary.pineapple.so` 在 LHDC 软解模式下误拦 BT SCO 路由（`-22`）的校验，零影响 LHDC/AAC 与杜比外放。
- **完整修复桌面图标「时间光影」**：合入 `TimeShadowFixHook`（`q5.g.F()` 解锁 + `IconStyleOverlayView.s1()` 光影渲染 + `IconSettingsFragment.j() -> k()` 修复开关错读 `Settings.System` 回弹），并在「图标」设置页底部正常显示生效。

## v2.6 (versionCode: 260)
- **个性化三件套全量解锁**：新增系统风格（圆角/壁纸吸色/基础色）、时间光影、OriginOS 7 图标风格（精美/简约/原生 16 色盘/质感/已下载 + 默认/开放图标）及 `/system/etc/theme/style/{7,8}` 遮罩支持
- **桌面体验增强**：解锁桌面文件夹 `2x1`（横向）与 `1x2`（竖向）自由切换；桌面设置新增「桌面显示搜索入口」独立开关与底部搜索胶囊
- **原厂 782.4 nits 背光曲线与真 HDR10 / 相册 XDR 对齐**：重构背光坐标系映射与 SDR/HDR 双通道白点分离，支持 HDR10/HLG 视频与相册 UltraHDR/XDR 照片高光激发，同时保证 SDR 界面与文字不刺眼、开机锁屏零过曝零花屏
- **原生硬件级直驱供电（旁路充电）**：打通游戏魔盒与电池设置「直驱供电」开关联动充电 IC (`charging_enable`)，并补齐 SELinux 权限消除 AVC
- **满血游戏性能与联想动态温控联动**：新增 SukiSU WebUI 四档调度面板（智能自动 / 日常省电 / 均衡稳帧 / Monster 满血），Monster 模式自动联动 4224MHz LPDDR5X 满血带宽与联想 `case 300` 游戏温控阈值，纯事件驱动零后台轮询
- **联想原厂相机 (`com.zui.camera`) 深度修复**：修复 `2x/4x/8x` 变焦拍照 `SIGILL` 闪退、人像追踪异常及物理微距镜头 (`Device 2`) 调用
- **酷安 / B站等第三方应用防闪退与防吞草稿 (`SPSGuard`)**：修复 OriginOS 7 后台管控 (`com.vivo.pem` / `SuperProcessSystem`) 每 5 分钟误调 `forceStopPackageWithoutCancelNotification` 静默强杀前台打字应用及清空草稿的原生 Bug
- **自动同步 LSPosed/Vector 作用域**：刷入模块自动同步新增推荐作用域（`com.bbk.theme`、`com.vivo.gamecube`、`com.vivo.pem`、`com.vivo.sps` 等），内置 `TB710FU_OOS7_AllInOne_v3.8.apk` 与 `TB710FU_DolbyAtmos.apk` 自动覆盖升级（2026-11-25）

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
