# Claude routing review — 2026-11-20

Reference: https://ip.net.coffee/claude/site.html

## Changes

- Keep all 21 existing service/auxiliary rules in `Claude.list`.
- Append `160.79.104.0/21`, `2607:6bc0::/32` and ASN `399358` as supplemental destination rules with `no-resolve`.
- Collect 11 shared-service suggestions in `Claude-Shared-Optional.list`. This file is not referenced by the active Trowoy configuration.
- Preserve Apple/Siri Gateway overrides, Chinese AI DIRECT rules, AI policy membership, manual selection and REJECT behavior. No Worker/KV/client-setting change is needed for this repository update.

## What the page already covers

All seven core suffixes in the reference already exist in the personal list. The existing `anthropic.com` suffix includes API, console, CDN and MCP subdomains; adding each subdomain again would be redundant. The exact Auth0, Cloudflare, BunnyCDN, Ghost and two Datadog endpoints are also already present. Preserve the extra service suffixes that the personal list had before this review.

## Shared services

The optional file contains Sentry, Statsig, Datadog/Sentry/Sift keywords, Intercom, Fathom, Google Maps and Persona-related domains. These are shared by other applications. A keyword can also match a domain unrelated to its apparent vendor. The community page associates them with Claude, but it does not prove that every host is required by the current CLI, Desktop app or website.

The official CLI network table explicitly lists the two exact Datadog intake hosts already in `Claude.list`; it does not require routing every Datadog or Sentry hostname to AI. Leave the optional file unreferenced unless that broader scope is desired. If enabled, its policy belongs on the RULE-SET reference, not inside the file, and it must be placed before general matching rules. It inherits the selected AI policy's availability and rejection behavior.

## IP and ASN limits

ARIN registration records confirm the IPv4 block, the IPv6 block and ASN. Registration is not a statement that every address serves Claude requests, nor an exhaustive list of current CDN destinations. Retain domain matching first and treat these rules as supplements.

`no-resolve` skips resolving an as-yet-unresolved destination domain solely for IP matching. It does not perform or disable reverse DNS, stop all DNS requests, force packets through a tunnel, or guarantee detection of encrypted application traffic. ASN matching depends on the client's ASN database. Review address ownership and client support when maintaining these entries.

## NTP correction

NTP synchronizes time and does not return the client's timezone. We found no evidence in the official CLI network requirements for treating every NTP server as Claude traffic. The reference's claim that proxying NTP makes returned timezone information match the proxy region is incorrect. Do not add `geosite:category-ntp` to these Surge lists or change system NTP routing based on that claim. `geosite:` is also not native Surge rule-set syntax.

## Applying the update

The existing Claude.list URL is unchanged. In Surge, refresh the rule sets; updating the managed profile alone does not guarantee that its rule-set cache refreshes immediately. The configured interval remains 86400 seconds.

FlClash uses the existing Worker's converted rule-provider URL. Refresh its providers; if conversion has a cached last-good copy, allow its cache interval or retry later. A successful configuration download alone does not prove that every provider has refreshed. No new Worker deployment is required.

These rules do not prove that all iOS app traffic is captured. Existing Windows Claude application rules remain in the generated FlClash profile. No local Claude/Anthropic endpoint, DNS lookup or proxy connectivity probe was used in this review.

## Sources

- Community reference: https://ip.net.coffee/claude/site.html
- Official CLI requirements (read with a remote web tool): https://code.claude.com/docs/en/network-config
- ARIN IPv4 registration: https://rdap.arin.net/registry/ip/160.79.104.0
- ARIN IPv6 registration: https://rdap.arin.net/registry/ip/2607:6bc0::
- ARIN ASN registration: https://rdap.arin.net/registry/autnum/399358
- Surge rule-set syntax: https://manual.nssurge.com/rules/ruleset.html
- Surge rule/DNS semantics: https://manual.nssurge.com/rules/overview.html
- NTP protocol, RFC 5905: https://www.rfc-editor.org/rfc/rfc5905

## Validation

Offline checks preserve the existing domain matches, keep shared-service rules out of the default list, validate CIDR/ASN values, and round-trip both lists through the existing FlClash classical converter. Both installed official Mihomo cores (1.19.33 and 1.19.34) check isolated inline-provider profiles with listeners, TUN and DNS disabled. These checks validate syntax and routing scope, not real service connectivity. The first native syntax check downloaded its ASN database automatically; it did not contact a Claude/Anthropic host.
