# Notes 📒  

The below is a general usage guide for AdGuard Home (AGH).  
**NOTE!** This is based on my experience. The below is not associated with the AGH team in any way. 
Please assess what is best for your own network and needs.

## Requirements ✅  

- AdGuard Home v0.107.72 or later recommended
- Run it on a device that is always on (Pi, NAS, mini PC)
- If you are familiar with Containers, install AGH in one of those
- Only ONE DHCP/DNS server on the network. Disable the DNS
  service on your router if running AGH on a separate device,
  or point the router's DHCP DNS setting at the AGH machine

## Setup basics 🧰  

- Install: https://github.com/AdguardTeam/AdGuardHome/wiki/Getting-Started
- First-run wizard sets admin user, web port (default 3000
  during setup, 80 afterwards) and DNS port (53)
- Access the dashboard at http://<device-ip>:80

## Upstream DNS servers 🔼  

Choose providers that suit your priorities (speed vs filtering vs privacy):
- Quad9: tls://dns.quad9.net        (malware blocking, DNSSEC)
- Cloudflare: https://security.cloudflare-dns.com/dns-query  
- Don't use filtering services in your upstream servers (Cloudflare
for Families for example). Configure this filtering via AGH filters
and blocked services  
- Run a benchmark tool for your location if unsure (e.g. GRC's
Domain Name Speed Benchmark). Fastest on paper isn't always
fastest for you  
- Parallel vs Fastest IP address: AGH can query several upstreams in
parallel ("Load balancing" / "Fastest address"). I find Paralell 
requests is the best option.

## Bootstrap DNS servers 🥾  

Needed to resolve the hostname of your encrypted upstreams.
- Leave AGH's defaults unless they don't work
- Good fallbacks: 1.1.1.1, 8.8.8.8, 9.9.9.9

## DNS server configuration ⚙  

- Enable DNSSEC: yes (verify upstream supports it)
- Cache size: 4 MB+ is fine for most homes

## Encryption (optional but recommended) 🔒  

Settings > Encryption settings:
- Add a certificate (Let's Encrypt or buy one) to serve
  DNS-over-HTTPS/TLS to LAN clients that support it
- Without a cert, AGH still works fine as plain DNS internally;
  encryption mainly matters if clients roam or you forward upstream
  (upstream URLs like https:// or tls:// are encrypted regardless)

## Filters 📓  

Filters > DNS blocklists:
- Start lean: one good general list beats twenty overlapping ones.
  AGH's built-in "AdGuard DNS filter" + OISD Small or Hagezi
  Multi Normal are sensible combos
- Check the lists! Not all blocklists contain adult sites. Some
  provide separate lists (OSID NSFW for example)
- More lists ≠ better. Overlapping lists cause false positives
  and slow updates

Filters > DNS allowlists:
- Add curated allowlists here (plain ||domain^ syntax)
- Example: Microsoft business services allowlist
  (see ms-allowlist.txt in this repo)

Filters > Custom filtering rules:
- Use for ad-hoc exceptions with @@||domain^ syntax
- Scratchpad for newly discovered fixes; promote tested ones
  into a hosted allowlist file

## Filters update interval ⏱  

Settings > General settings > "Filters update interval"
- 12 hours is a good balance for home use

## Blocked services 🚨  

Filters > Blocked services:
- Easiest way to block entire services (TikTok, etc.)
- Block all, then toggle OFF services you legitimately use
- Careful: blocking Microsoft or Google services here WILL break
  business tooling regardless of your allowlists. The blocked
  services feature works separately from DNS blocklists

## Query log hygiene 🧹  

Settings > DNS settings > "Disallowed domains":
- Paste TLDs/domains you never need to see (e.g. ssl.google-analytics.com)
  to silence query-log spam
- Daily habit for the first week: Query Log, filter "Blocked",
  unblock anything legitimate that got caught. This is how you
  build YOUR allowlist

## Clients 👥  

- Per-client settings let you give kids' devices stricter
  filtering than your work laptop
- Fixed IPs + DHCP reservations make client rules reliable

## Testing & troubleshooting 🔍  

- Command-line check: nslookup doubleclick.net <AGH-ip>
  should return 0.0.0.0 (blocked); a normal site should resolve
- If something breaks, FIRST check the Query Log before blaming
  the app. Filter by the domain or "Blocked" status
- Safe-mode test: disconnect AGH from DNS duties temporarily;
  if the problem persists, it's not AGH
