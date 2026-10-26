# REPOSITORY SCOPE

Generic project; no specific hosting target confirmed. Do not apply cPanel assumptions until confirmed.

# AGENT OPERATING POLICY — cPanel Singapore, 26 Oct 2026

**New canonical rule:** Owner migrated hosting account `clzwvasf` from IDPanel to IDCloudHost **cPanel Singapore** on 26 October 2026. For websites that actually run in that account, this supersedes older IDPanel/LSAPI-specific deployment instructions in README/history. This commit is a documentation notice; it does **not** establish live deployment or change hosting configuration.

## Hosting migration rules (apply only to hosted projects)
- cPanel controls: Domains, MultiPHP Manager/INI Editor, Setup Node.js App (CloudLinux Passenger), Cron Jobs, Terminal, SSL and File Manager. Always verify domain registration, document root and actual server state; AUTO MAXCOOL and hq were initially missing from the new cPanel Domains list.
- HTTP PHP and CLI versions are **per application**. IDC reported PHP **8.4.27** for main supported web projects, PHP **8.2.35** on cs.csplus.id, esp32.csplus.id and shop.csplus.id at cutover. Do not force every repository to PHP 8.4 without compatibility testing; avoid obsolete 8.1. Validate real runtime, extensions and per-domain settings.
- PHP effective HTTPS `ini_get()` minimums: `upload_max_filesize=64M`, `post_max_size=128M`, `memory_limit=256M`, `max_execution_time=300`, `max_input_time=300`. Set domain values through cPanel MultiPHP INI Editor (or a verified host-supported configuration). **Do not use presence of literal `php_value` blocks in `.htaccess` as a release gate.** The old mandatory IDPanel LiteSpeed/LSAPI `php_value` instruction is deprecated. Preserve the live `.htaccess` and Passenger/handler routing, regardless of what a package contains; do not remove any rule blindly.
- Retain `.env`, `php.ini`, uploads, storage/private, DB/data/users/passwords, email accounts, DNS, SSL, cron, Node environments, WhatsApp credentials/auth, active `.htaccess`, and audit logs. Never commit runtime credentials or backup databases. Do not change live PHP, domains, or configs merely to update documentation.
- Check cron executable path/extension compatibility on cPanel; the old `/opt/alt/php84/usr/bin/php` path may or may not work after cutover. CloudLinux Passenger may sleep; use app-specific safe wake/heartbeat and UAT, not assumptions about uptime.

## Mandatory development/release workflow
1. Inspect live/deployed version separately from GitHub HEAD. Read existing code, routes, migrations, and security; preserve locked UI/SEO/Telegram/approvals. Implement minimal fixes and test locally.
2. Sync source to **this** GitHub repository with read-back commit verification; do not run GitHub Actions, create new CI publishers, or require GitHub Releases for ordinary release distribution (the owner wants to avoid GitHub Actions limits). Do not alter existing Actions files as a side effect of this notice.
3. Build a versioned **FULL ROOT CLEAN ZIP** in ChatGPT, with file manifest and real SHA-256; verify source/package match. No secrets, auth sessions, or live uploads inside. Owner downloads ZIP **directly from chat**, not GitHub Releases.
4. Owner deploys via the project's **Health Center** ZIP installer. This is the normal deployment route; do not demand manual cPanel SQL/extract. Run **only new migrations** needed for that project and verify Health Check. Rare one-time bootstraps require backups, narrow scope and explicit reason.
5. Do project-relevant live UAT (login, payments, bookings, invoice, Telegram, WhatsApp, cron, file uploads, SEO); declare separately `code synced`, `ZIP verified`, `live deployed`, and `end-to-end tested`. Never claim PASS from source presence alone. Protect rollback and report limitations.

**No automatic production action is authorized by this policy.** If deployment is not confirmed on the hosting account, treat the cPanel details as background only; preserve the existing runtime/workflow.
