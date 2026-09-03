# AdGuard Home — Microsoft Services Allowlist

A curated DNS allowlist for [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome)
that prevents filtering from breaking Microsoft business services:

🏢 Teams  
📧 Outlook  
☁️ Azure  
🧑‍💻 Azure DevOps  
🫙 OneDrive  
🌐 SharePoint  
♻️ Intune  
📲 Microsoft Authenticator and friends  

If you run aggressive blocklists at home (especially if you work from home), you've
probably noticed Microsoft 365 login loops, Teams calls failing, or Authenticator
getting stuck. This list fixes that, while keeping general ad/tracker blocking intact
for everything else.

## What's covered

| Service | Notes |
|---|---|
| Authentication & identity | login.microsoftonline.com, msidentity.com, Azure AD / Entra endpoints |
| Microsoft Teams | Including CDN and media endpoints |
| Outlook / Exchange | Office 365 mail endpoints |
| OneDrive & SharePoint | Business and personal endpoints |
| Azure & Azure DevOps | Management plane, blob storage, dev.azure.com, visualstudio.com |
| Intune / Endpoint Manager | Company Portal, device management, enrollment |
| Power Platform | Power Apps, Power Automate |
| Windows Update & Defender | OS servicing, SmartScreen |
| Mobile push (iOS/Android) | Apple APNs and Google FCM endpoints required by Teams/Authenticator |

## Installation

1. Copy the raw URL for the list file:

https://raw.githubusercontent.com/picklesnicholls/adguard-ms-allowlist/refs/heads/main/ms-allowlist.txt


2. In AdGuard Home, go to **Filters → DNS allowlist → Add filter**.
3. Paste the URL, give it a name (e.g. "Microsoft Services"), and save.
4. Optionally force an update on the new filter to pull it immediately.

### ⚠️ Syntax note — read this

This file is written for **allowlist filters only** (rules like `||domain^`).
Do **not** paste these rules into **Custom filtering rules** — that location
expects *exception* rules, which look like `@@||domain^` instead. If you mix
them up, the rules won't do anything.

## Telemetry disclaimer

Some entries (`events.data.microsoft.com`, `aria.microsoft.com`, and a few
others) are Microsoft diagnostic/telemetry endpoints. They are included because
some Office features behave oddly without them. **If you'd rather keep
telemetry blocked, simply delete those lines** — Teams and Office work fine
without them for most users. The sections are commented so you can identify
them easily.

## Why Apple & Google domains?

Microsoft Teams and Authenticator on iPhone and Android depend on platform
push-notification services (Apple APNs, Google FCM). Aggressive blocklists
sometimes catch these, resulting in missed calls and silent notifications even
though the apps themselves look fine. Those entries are grouped separately so
you can review or remove them.

## This list won't cover everything

Microsoft migrates endpoints over time (increasingly towards `.microsoft` and
`.cloud.microsoft` TLDs). If something is still broken after installing:

1. Open your AdGuard Home **Query Log**, filter for *Blocked*, and look for
   domains containing `microsoft`, `azure`, `office`, `msidentity`, etc.
2. Add the domain to the list (or temporarily unblock it via the Query Log).
3. Please open an issue here with the domain so everyone benefits!

## Contributing

Issues and PRs welcome. When reporting a domain, please include:
- the exact hostname from your Query Log,
- which app/service was misbehaving,
- and (if known) the blocklist that caught it.

## License

MIT — see [LICENSE](LICENSE). Provided as-is, with no guarantee of completeness.
