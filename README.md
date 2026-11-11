# Trowoy Rules

Personal supplements for Surge. These files contain public domain names only.

- `Claude.list`: priority Claude/Anthropic service domains and exact documented auxiliary endpoints. Place first and route to AI.
- `AI-CN.list`: direct-routing overrides for Chinese AI providers, including international domain aliases. Load before all overseas AI sets.
- `AI-Global.list`: narrowly scoped overseas AI endpoints missing from the reviewed community snapshots. Route to the AI policy.

## Suggested order

```ini
RULE-SET,https://raw.githubusercontent.com/Trovoy/Trowoy-Rules/main/Claude.list,AI,extended-matching,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/Trovoy/Trowoy-Rules/main/AI-CN.list,DIRECT,extended-matching,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/MetaCubeX/meta-rules-dat/meta/geo/geosite/classical/category-ai-cn.list,DIRECT,extended-matching,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/Trovoy/Trowoy-Rules/main/AI-Global.list,AI,extended-matching,update-interval=86400
RULE-SET,https://ruleset.skk.moe/List/non_ip/ai.conf,AI,extended-matching,update-interval=86400
RULE-SET,https://raw.githubusercontent.com/hoobnn/fly-rule/release/metacubex/geosite/surge/category-ai-!cn.conf,AI,extended-matching,update-interval=86400
```

The final source converts MetaCubeX rules to Surge syntax, dropping unsupported DOMAIN-REGEX entries. Community sources maintain their own licenses and update schedules. They are referenced, not copied into this repository.

Edit the appropriate list to add a domain. Keep Surge rule syntax, omit policy names inside the lists, and avoid broad shared CDN, login, telemetry, or keyword rules. Changes take effect after Surge refreshes its rule sets; the interval above is 24 hours. Main profile refresh and rule-set refresh are separate client operations.

Chinese services use DIRECT under this policy even when a community overseas set includes their international aliases. Global products such as Coze.com and Trae.ai follow the overseas sets; Coze.cn and Trae.cn follow the Chinese set. This is an explicit routing preference, not a guarantee of service availability from every network.

No subscriptions, proxies, certificates, or tokens belong in this repository. There is no scheduled generation: personal lists change when edited; community sets update independently.

## Claude priority and limits

`Claude.list` is loaded before LAN, manual and general routing rules in Trowoy. It explicitly covers service domain suffixes, so new subdomains under those suffixes also match. It includes API, authentication, downloads, file/Artifact and MCP service domains independently of community coverage. Two exact Datadog hostnames listed in Anthropic's network documentation are included; these hostnames may also receive unrelated applications' telemetry.

Sources: https://code.claude.com/docs/en/network-config and the reviewed MetaCubeX Anthropic domain snapshot. Validation uses offline rule matching, not requests to Claude services. This does not identify all traffic from an iOS app: shared CDN/login endpoints, arbitrary external MCP servers, and future unrelated domains cannot be reliably attributed to Claude by domain-only rules. Per-app full-traffic enforcement requires a supported device/per-app VPN policy or manual global routing to AI during use. It must not be claimed as achieved by this domain list alone.
