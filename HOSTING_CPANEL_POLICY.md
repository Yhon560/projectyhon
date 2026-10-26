# Notice — GitHub working agreement and cPanel Singapore infrastructure
**Effective 26 October 2026 · Account: Yhon560 · Status: POLICY/DOCUMENTATION (not a production deployment)**

## Hosting change
The user's CSPLUS-family **web hosting account** migrated from **IDCloudHost IDPanel / CloudLinux LiteSpeed** to **IDCloudHost shared hosting on cPanel Singapore** on 26 October 2026. This replaces the *control-panel/deployment assumptions* in older docs; it does not automatically migrate every repo or verify every subdomain. Files, MySQL databases, cron entries, SSL, DNS, and mail were reported migrated; application verification is required before claiming live PASS.

### Current cPanel/production constraints
1. Configure PHP **per domain** using **cPanel MultiPHP Manager / MultiPHP INI Editor** as supported by the active PHP handler. Do not assume IDPanel selectors, legacy `.user.ini`, or `php_value` blocks in `.htaccess` are the authoritative setting. The effective limits must be checked via `ini_get()` in an **HTTPS web request**:
   - `upload_max_filesize=64M`
   - `post_max_size=128M`
   - `memory_limit=256M`
   - `max_execution_time=300`
   - `max_input_time=300`
2. **PHP 8.4.27** was reported available on the new host, but **do not upgrade every site indiscriminately**. Verify the app's version policy and runtime dependencies first. Some migrated domains remained on PHP 8.2.35. Web PHP and **PHP CLI** can be different; discover the actual CLI path on cPanel before touching cron commands. Never assume legacy `/opt/alt/php84/usr/bin/php` still exists.
3. **Preserve live configuration** on any release: `.env`, private secrets, root `.htaccess` with cPanel/Passenger routing, per-domain `php.ini`, `storage/private`, customer uploads, MySQL data/users/passwords, cron, mail, SSL and webhook endpoints. Never overwrite live `.htaccess` with a stale repository copy. When a host-managed setting is wrong, resolve it in cPanel or with hosting rather than silently modifying app/handler settings.
4. Never put secrets, session credentials, API tokens, passwords, customer data or server backups into GitHub, ZIP assets, logs or chat.
5. CloudLinux Passenger Node.js is available **for applications configured to use it**. A running HTTP endpoint does not itself prove a persistent backend connection or a successful WhatsApp send. Validate heartbeat, connection state, real message, and reconnect; preserve sessions and auth data.

## Updated GitHub → ZIP → Health Center workflow (for web apps that implement it)
1. Read current GitHub default branch/source and the real production release baseline. Audit code, migration history, security, and rollback before any change.
2. Make minimal backward-compatible edits in the correct repository. Preserve existing business workflows, customers, Telegram, databases, and links. Do not rewrite multiple repos just to match this notice.
3. Run syntax checks, focused tests, security and regression checks. No success claim without evidence. GitHub Actions are **not required for this policy**; do not create/enable noisy automatic publishing or Actions merely to make a release.
4. Synchronize the authoritative source to GitHub and perform a read-back check of commits. Make a **FULL ROOT CLEAN ZIP** plus SHA-256 manifest/checksum **when the particular project already supports that release contract**, with no secrets or customer data. Do not make package claims unless the archive was actually built/verified.
5. Prefer each web app's **existing, secured Health Center** for deploy/migrations; each project has its *own* routes, schema and rules. Do **not** blindly upload/extract the ZIP via cPanel File Manager. If an app lacks a Health Center, use its established deployment SOP—do not invent one.
6. Make a recoverable backup first; deploy to the correct document root; run **only new unapplied migrations** and never rerun SQL manually without cause. Preserve live cPanel handler/Passenger settings.
7. Verify HTTPS web PHP/version/extensions/limits, DB, admin login, invoices/payments/forms where relevant, cron, notifications, WhatsApp/Telegram state, SSL and affected user flows. Distinguish **source synced**, **ZIP verified**, **deploy applied**, and **live UAT PASS**.
8. If trouble arises, stop potentially destructive changes, retain evidence, and use a tested rollback plan. Do not delete DNS records, `.htaccess`, `nodevenv`, sessions or existing customer records to troubleshoot.
9. Do not claim a GitHub Release was published unless its asset/link/checksum were verified. User prefers **a direct downloadable ZIP** and avoids unnecessary GitHub Actions usage.

## Repository-specific scope
- Lightweight project repository. This notice documents the account-level infrastructure standard; no claim is made that this repository is deployed on cPanel. No application deployment change authorized.

## Priority and history
This notice supersedes earlier **IDPanel-specific infrastructure instructions** only. It does not supersede business logic, releases, existing compatibility requirements or product security gates. Document future environment changes here, in the project README, and in release notes when applicable.
