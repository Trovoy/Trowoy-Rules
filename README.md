# Trowoy Rules

Personal supplements for Surge. These files contain public domain names only.

- `Apple-Siri.list`: all 16 user-selected Apple/Siri/iCloud/ChatGPT/OpenAI overrides, routed to Gateway. Load immediately after Claude and before normal AI/Apple/DIRECT rules.
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

## Explicit Gateway overrides (2026-11-11)

Per the user's latest preference, all 16 entries in Apple-Siri.list route to Gateway, including chatgpt.com and openai.com. These take precedence over the general AI sets and Apple domestic/CDN DIRECT rules. Claude remains the highest-priority AI set. Other OpenAI-related domains, such as oaiusercontent.com, retain their existing policy unless explicitly added here; this list changes exactly the supplied rules, not every related service dependency.

The siri keyword and icloud-content.com suffix intentionally preserve the supplied matching scope. The keyword matches any domain containing siri; the iCloud suffix also covers non-AI content. These are explicit user overrides, not an Apple Intelligence-only detector. A name-specific host such as p150-acsegateway.icloud.com matches only that host, not every numbered gateway.

Policies live in the private Surge profile's RULE-SET references, not inside these public lists. Edit Apple-Siri.list to maintain these entries; a list-only edit requires a Surge rule-set refresh, not a Worker deployment.

`Apple-Siri.list` is the canonical filename. `Gateway.list` remains a legacy snapshot so profiles that have not refreshed their reference continue to work. Refresh the managed profile to use the canonical URL. Rule content and Gateway policy are unchanged.
