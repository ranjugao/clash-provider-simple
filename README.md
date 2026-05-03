# clash-provider-simple

A small rule-provider aggregation repository for Clash Mi on iOS.

This repository keeps Clash Mi stable by moving rule-provider updates out of the iOS client and into GitHub Actions. Clash Mi only needs to load three merged rule files instead of many upstream providers, reducing startup-time HTTP requests, DNS lookups, and Network Extension timeout risk.

## Output Files

- `merged_proxy.txt`: `proxy.txt` + `gfw.txt` + `tld-not-cn.txt` + `google.txt`
- `merged_direct.txt`: `direct.txt` + `cncidr.txt` + `lancidr.txt` + `apple.txt` + `icloud.txt`
- `merged_reject.txt`: `reject.txt` + `private.txt`

## Raw URLs

```text
https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_proxy.txt
https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_direct.txt
https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_reject.txt
```

## Usage

Replace many upstream rule providers in Clash Mi with these three merged providers:

```yaml
rule-providers:
  proxy:
    type: http
    behavior: domain
    url: https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_proxy.txt
    interval: 21600

  direct:
    type: http
    behavior: domain
    url: https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_direct.txt
    interval: 21600

  reject:
    type: http
    behavior: domain
    url: https://raw.githubusercontent.com/ranjugao/clash-provider-simple/main/merged_reject.txt
    interval: 21600

rules:
  - RULE-SET,reject,REJECT
  - RULE-SET,direct,DIRECT
  - RULE-SET,proxy,PROXY
```

If your proxy policy group is not named `PROXY`, replace `PROXY` with the actual policy group name in your Clash Mi profile.

## Update Schedule

GitHub Actions runs every 6 hours and can also be triggered manually from the Actions tab.

The update workflow:

1. Downloads upstream rule files from `Loyalsoldier/clash-rules`.
2. Merges them into three provider files.
3. Removes empty lines.
4. Deduplicates entries with `sort -u`.
5. Commits and pushes only when the merged files changed.
