# Quantumult X 规则

## Bank

`Bank.list` 是来自 [yeahwu/Rules-For-Quantumult-X](https://github.com/yeahwu/Rules-For-Quantumult-X/blob/main/Rules/Services/Bank.list) 的本地规则副本，保留原有 22 条启用规则和 1 条注释规则。文件由本仓库自行管理，不会自动同步上游。

在 Quantumult X 配置的 `[filter_remote]` 段添加：

```ini
https://raw.githubusercontent.com/rederry/rules/main/QuantumultX/Bank.list, tag=Bank, force-policy=你的代理策略组名称, enabled=true
```

将 `你的代理策略组名称` 替换为已存在且实际使用代理节点的策略组或节点名称。`force-policy` 会覆盖规则中的 `Bank` 策略。也可不写 `force-policy`，并在本地定义名为 `Bank` 的策略组且选择代理节点。请在 Apple、HSBC、银行应用的相关规则可能重叠时，按需调整规则优先级。

## Apple Intelligence / Siri

`AppleIntelligence.list` 是自行维护的规则快照，包含上游 5 条精确域名规则，以及本地补充的 `api-siri-prod.apple.com`、`smoot.apple.com` 后缀规则和 `amp-api.apps.apple.com` 精确域名规则，共 8 条。后续直接修改本文件，不会自动跟随上游更新。

上游来源：<https://raw.githubusercontent.com/enriquephl/QuantumultX_config/main/filters/AppleIntelligence.conf>（读取日期：2026-10-19）。原链接的 `#via=0` 是 URL 片段，不属于规则内容，无需加入订阅地址。

### 引用方式

将下面一行加入 Quantumult X 现有配置的 `[filter_remote]` 段，不要重复创建该段：

```ini
https://raw.githubusercontent.com/rederry/rules/main/QuantumultX/AppleIntelligence.list, tag=AppleIntelligence, force-policy=你的代理策略组名称, enabled=true
```

将 `你的代理策略组名称` 替换为已经存在的代理策略组或节点名称，并确认该组实际选中可用的代理节点。`force-policy` 会覆盖文件第三列的 `AppleIntelligence`，因此此方式无需另外创建同名策略组。不要设置为 `direct`，也不要在所选策略组中选中直连。

如果已有名为 `AppleIntelligence` 的策略组且已选中代理节点，可以省略 `force-policy`：

```ini
https://raw.githubusercontent.com/rederry/rules/main/QuantumultX/AppleIntelligence.list, tag=AppleIntelligence, enabled=true
```

上述在线地址只有在你自行将文件提交并推送到 GitHub 的 `main` 分支后才会包含本次修改。本次仅修改本地文件，未提交、未推送。需要立即测试时，可将 `.list` 中 7 条 `HOST` / `HOST-SUFFIX` 规则复制到现有 `[filter_local]` 段，将第三列统一替换成已有的代理策略组或节点名称。

### 优先级和验证

- 使用规则分流模式；将本规则安排在 Apple 域名直连规则之前，并检查本地规则是否提前将这些域名设为直连。避免已有同域名规则产生冲突。
- 更新远程规则资源并重新连接 Quantumult X 后，触发一次 Siri 云端请求，在请求记录中检查 `api-siri-prod.apple.com` 是否命中本规则且实际连接使用代理节点。
- 本仓库检查仅覆盖规则格式、去重和差异；是否恢复 Siri 功能仍需在 iPhone 上验证。

Quantumult X 的远程规则和 `force-policy` 语法参考：[官方示例配置](https://github.com/crossutility/Quantumult-X/blob/master/sample.conf)。
