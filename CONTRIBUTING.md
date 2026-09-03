# Contributing

## Adding a domain
- One rule per line, `||domain^` format (this list targets AdGuard Home
  *allowlist filters*, not custom filtering rules).
- Group it under the relevant comment section, or create one if none fits.
- Make sure it's genuinely required for a service to function, not just
  cosmetic.

## Reporting an issue
Include the full hostname from your Query Log, the affected app, and your
device/OS. Vague "Teams is broken" reports are hard to act on.

## Style
- Alphabetise within sections.
- Use `!` comment lines for section headers.
- Flag anything telemetry-related with a comment so users can remove it easily.
