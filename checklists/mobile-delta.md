# 移动端平台差异点（iOS vs Android）

> 被 `architecture.md` / `code.md` 的「移动端专项」引用：审核对象涉及原生层（权限/推送/后台/签名/分包）时读取本文件。
> 仅收录**双端不同**的点，共性检查仍在原清单。

## 架构层差异

| 维度 | Android | iOS | 审核要点 |
|---|---|---|---|
| 推送通道 | FCM / 厂商通道（小米/华为/OPPO…） | APNs 唯一通道 | 多通道 fallback 是否画清；厂商通道缺失时是否降级 |
| 原生-Flutter 通信 | MethodChannel / Pigeon / JNI | MethodChannel / Pigeon | 双端接口是否对称；Pigeon 契约是否单源生成 |
| 热更新合规 | 相对宽松（应用宝/官网包） | App Store 禁止动态下发可执行代码 | 热更新/动态化方案是否过审；企业包与商店包策略是否分离 |
| 后台机制 | Service / WorkManager（各厂商裁剪不一） | Background Modes（受限，后台刷新/静默推送） | 后台任务在双端的存活预期是否写明；厂商白名单/特殊配置 |
| 环境映射 | `productFlavors` + `applicationIdSuffix` | Scheme / Target + BundleID | flavor↔Scheme 映射表是否存在；包名/BundleID 规则是否一致 |

## 代码层差异

| 维度 | Android | iOS | 审核要点 |
|---|---|---|---|
| 权限申请 | 动态权限（Android 6+，部分需二次弹窗解释） | Info.plist 用途描述必填，缺 key 直接 crash | 双端权限矩阵是否列全；拒绝/永久拒绝路径是否有降级 UI |
| 签名配置 | keystore（debug/release，CI 托管） | 证书 + Provisioning Profile（development/ad-hoc/app-store） | 签名资产归属与过期告警；flavor/scheme 与签名组合矩阵 |
| 分包与体积 | ABI split（arm64-v8a/x86_64…）+ density | App Thinning / Bitcode（历史）/ Slicing | 分包策略与产物命名（`app-<abi>-<flavor>-<mode>.apk` vs `<Scheme>.app`）；超大包预警线 |
| 最低版本 | minSdkVersion 碎片化（API 21~34） | Deployment Target（通常近 3 代） | 双端最低版本对应的 API 可用性；废弃 API 的条件编译 |
| 生物识别/安全存储 | BiometricPrompt + Keystore/StrongBox | FaceID/TouchID + Keychain/Secure Enclave | 回调对称/迁移策略/降级路径 |
| 应用内更新/强更 | Play InAppUpdate + 热修复 | 仅 App Store 强更 / 热更合规风险 | 触发条件一致/合规评估 |

## 原生稳定性差异

| 维度 | Android | iOS | 审核要点 |
|---|---|---|---|
| 崩溃/ANR 采集 | tombstone + ANR traces + Play Console | .ips/.crash + Metrickit/Crashlytics | 管道统一/符号表自动化/阈值对齐 |
| 深度链接 | intent-filter + autoVerify + assetlinks | Associated Domains + AASA | 域名验证/回退 Scheme/测试矩阵 |

## 输出要求

遵循 `SKILL.md` 步骤4 与「输出纪律」：✅ 项合并一行，仅 ⚠️/❌/❓ 展开，每条格式：

`[P?·真实缺陷|加固建议] 位置 - 问题 - 证据<E?> - 建议动作 - 影响/触发条件`
