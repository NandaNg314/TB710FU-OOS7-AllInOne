# 更新日志（TB710FU OriginOS 7 全能修复）

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
