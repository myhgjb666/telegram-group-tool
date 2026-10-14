# Telegram Bulk Message Sender — Desktop Group Broadcast Tool

[中文](README.md) ｜ **English**

A multi-account **Telegram bulk message / mass DM** desktop client for Windows. It sends to groups you have **already joined**, with message templates, scheduled plans and delivery logs — built for community announcements and routine reminders.

**Keywords:** telegram bulk message sender, telegram bulk messaging, telegram mass message tool, telegram mass DM software, telegram group broadcast tool, telegram multi-account sender, telegram auto message sender, telegram message automation, telegram marketing tool, telegram bulk sender for Windows, send bulk messages to telegram groups, telegram group messaging software, telegram broadcast desktop app, TG bulk sender, Telegram 群发 / TG 群发工具 / 电报群发.

> Latest version: **v0.2.28** (Windows)  
> This repository hosts the installer and usage documentation for easy download. Source code is not included.

---

## Download

| Version | File | Notes |
|---|---|---|
| **v0.2.28** (latest) | [tg-broadcast-v0.2.28.exe](https://github.com/myhgjb666/telegram-group-tool/releases/download/v0.2.28/tg-broadcast-v0.2.28.exe) | Windows 64-bit single file, run directly |
| v0.2.27 | [tg-broadcast-v0.2.27.exe](https://github.com/myhgjb666/telegram-group-tool/releases/download/v0.2.27/tg-broadcast-v0.2.27.exe) | Previous release |

Older builds are available on the [Releases](https://github.com/myhgjb666/telegram-group-tool/releases) page.

**Requirements:** Windows 10 / 11 (64-bit)

If your antivirus blocks the first launch, choose "Run anyway" or add an exception — the tool is a PyInstaller-packed desktop app and is prone to false positives.

---

## Features

- **Multi-account management** — add several Telegram accounts, check status, daily sent count and daily quota
- **Group picker** — sync your joined groups and tick the ones you want to send to
- **Message templates** — multiple variants with rotation and variables (date, time, group name, random string)
- **Scheduled plans** — cron or fixed interval, run now / pause anytime
- **Anti-ban pacing** — configurable cooldown, daily quota, daytime window and jitter
- **Delivery log** — success / failure reasons, per-message duration, searchable and paginated
- **Keyword listener** — auto-reply to group messages or private chats when a keyword matches, with cooldown and daily cap
- **Public group search** — search public groups / channels by keyword, join at a limited rate and add them as targets
- **Restriction center** — Telegram flood-wait countdown and official @SpamBot restriction status

---

## Screenshots

### Sign in

![Telegram bulk message sender login screen](screenshots/04-login.png)

### Accounts

![Telegram multi-account sender — accounts management](screenshots/01-accounts.png)

### Groups

![Telegram group broadcast — target group picker](screenshots/02-groups.png)

### Scheduled plans

![Telegram scheduled bulk messaging plans](screenshots/03-schedules.png)

### Add a Telegram account

![Add a Telegram account for bulk messaging](screenshots/05-add-account.png)

---

## Quick start

1. Download and run `tg-broadcast-v0.2.28.exe`
2. Sign up / sign in to the app account (new devices usually get a trial period, then a license key is needed)
3. Open [my.telegram.org/apps](https://my.telegram.org/apps), create an application and copy your `api_id` / `api_hash`
4. On the **Accounts** page, add a Telegram account (API credentials + phone number, then the login code / 2FA password)
5. On the **Groups** page, discover your groups and tick the targets
6. Create content and a plan under **Templates** / **Schedules**, then start sending

---

## Changelog

### v0.2.28 (current)

- **A failed "cannot securely connect to the licensing service" sign-in now tells you why.** Previously it only said "certificate verification failed", which did not distinguish an expired certificate from a hijacked connection. The app now inspects the certificate actually presented, and reports one of three cases: ① unknown issuer or hostname mismatch → the encrypted connection was replaced by a VPN / proxy / corporate gateway; switch nodes or let the licensing host bypass the proxy; ② certificate expired → update to the latest build; ③ certificate is valid but local verification still fails → usually a **wrong system clock** or an outdated local root store; sync your time first
- **Error messages no longer reveal the licensing service domain**, to avoid exposing the backend address
- The message still keeps the useful diagnostics (who issued the certificate, whether it matches the host) so problems can be pinpointed at a glance

### v0.2.27

- **Fixed groups being auto-blacklisted even though they can still be messaged.** Previously, if Telegram replied "you can't write in this chat" (usually a temporary restriction that lifts by itself), the app treated it as a permanent ban, blacklisted the group and unticked it — while you could still post there manually from the Telegram app. Temporary restrictions are no longer blacklisted; the send is only recorded as a failure and retried on the next round. Only a real ban (you were kicked / banned in that group) triggers a blacklist entry
- **New "unblacklist" action** — right-click any group in the list to blacklist it (stop sending) or unblacklist it (make it selectable again)
- **New bulk cleanup** — right-click an account or group row for "Unblacklist all (N groups)" to restore every wrongly blacklisted group of that account in one step
- Unblacklisting also clears the stale "muted" flag written by older versions (which greyed out the checkbox and made it unclickable); the real mute state is re-synced on the next group discovery
- The top-right counters and group tooltips now note "right-click to unblacklist"

### v0.2.26

- **Re-fetching groups or changing selections mid-run now takes effect immediately.** Previously the target list was snapshotted when a round started, so refetching groups or changing ticks had no effect until the next round (the displayed total stayed frozen at the value from the start). The plan now re-syncs the target list every 30 seconds — newly ticked groups join the current round, the total updates live, and unticked groups are skipped
- **More accurate "selected" count** — blacklisted groups no longer count as selected even if still ticked (they are never sent to). Rows show "selected X/Y · N blacklisted excluded" and the top-right counter shows "N blacklisted", removing the "shows 343 selected but only 328 are sent" confusion

### v0.2.25

- **Fixed the slowdown after long runs.** Network connections (especially relay routes) degrade after continuous use, but the underlying library only checks whether a connection is *alive*, not whether it has become *slow* — small packets still go through, so it believes the connection is fine forever and never rotates it; only manually reconnecting the proxy or restarting the app helped. The app now actively health-checks connections: a connection is rebuilt after roughly 25 minutes, consecutive slow sends also trigger a rebuild, and a timed-out send rebuilds the connection immediately before continuing
- **The "duration" column in the delivery log is now accurate** — it measures only the send itself, excluding pacing (cooldown, jitter), so it directly shows whether the network / proxy is degrading
- The status bar shows connection state: "oldest connection N min · N rebuilds", making automatic rotation visible
- Stability: connections now have a 15-second timeout, so a half-dead proxy no longer hangs silently

### v0.2.24

- **License key activation** — after buying a key, click "Already have a key? Activate / Renew" at the bottom of the sign-in window and enter your credentials plus the key to activate and extend usage in one step (expired accounts can self-renew, no admin needed)
- The sign-in window only shows "Sign in / Sign up for trial" by default, so new-user trials are not blocked
- Stability fix for an internal error that could prevent the sign-in window from opening

### v0.2.23

- The `{{random}}` template variable is now a **3-character letter+digit string** (e.g. `k7X`, `1gB`) instead of a variable-length number. Example: `Limited offer, join the group for resources {{random}}`
- New custom-length form: `{{random8}}` produces 8 characters, `{{random1}}` produces 1 (range 1–64)
- Purpose: make every send of the same copy slightly different, lowering the chance of being flagged as "duplicate content" (and thus "skipped as duplicate")
- Tip: this only reduces exact duplication — putting variables into the body (e.g. `{{group_name}}`) is more effective than appending a random suffix

### v0.2.22

- **Fixed "fast at first, slower after about half an hour"** — caused by Telegram rate limits on repeated broadcast content being **silently swallowed**: the library used to sleep through flood waits under 60 seconds and retry without logging anything, so the pacing stretched invisibly. Such waits are now logged (including the seconds needed) and the account shows a temporary cooldown countdown
- New send timeout: a single send waits at most 120 seconds if the network / proxy stalls, then counts as a failure and moves on — no more long "stuck with an empty log" periods
- **New "duration" column in the delivery log** — see how long each send actually took, to tell a slow network from rate limiting
- If your account hits limits often, slow the pace and diversify the copy (more variables, more variants) rather than just shortening the time window

### v0.2.21

- **Fixed pacing reverting after restart** — group cooldown, jitter, active hours and the duplicate-content switch previously applied only to the current run and silently reverted to conservative defaults (60 s cooldown, 10–60 s jitter) on restart or update. They are now persisted and restored on startup
- New "duplicate content window" setting (default 300 s): the same copy is sent to only one group within the window, to avoid spam flags. Lower it to speed up static-copy broadcasts (faster, but higher risk)
- The status bar now shows the cooldown and active hours actually in effect instead of fixed defaults

### v0.2.20

- Fixed settings loss after updating: data (settings, accounts, groups, templates) is now stored in a fixed system data directory (`%LOCALAPPDATA%\tg-broadcast`) instead of next to the executable, so moving the folder or updating never loses it
- Existing users get an automatic one-time migration from the old location (the old files are kept as a backup), no action needed

### v0.2.19

- Fully addressed the slowdown after long-term use: the database now maintains itself, periodically purging expired logs, preventing unbounded growth and reclaiming disk space on demand
- Log bodies are stored in a slim form (successes keep no body, failures keep a summary), cutting database size substantially
- New "Compact database" button on the log page for existing databases

### v0.2.18

### v0.2.17

- Stability improvements across group and account flows
- Various UI detail and wording improvements

### v0.2.16

- Stability and wording improvements for group discovery
- Streamlined the settings page, removing one redundant option

### v0.2.15

- **Private-chat auto reply** — listener rules can target group messages only, private messages only, or both; fixed group rules wrongly replying in private chats
- **Public group harvesting** — search public Telegram groups / channels by keyword, join a limited number at a throttled rate and set them as targets (member lists are not collected)
- **Restriction center** — a "Restriction" column on the accounts page showing the Telegram flood-wait countdown, with official @SpamBot status lookup and manual clearing of local limits

### v0.2.14

- Fixed database connection-pool exhaustion during multi-account parallel sending ("Stop plan" and similar actions reported pool exhaustion)
- "Stop plan" now takes effect immediately: the scheduled job is unregistered, in-flight sends are cancelled and the plan is persisted as disabled

### v0.2.13

- New "Clear sending restrictions" action on the accounts page
- "Sent today" now follows the anti-ban counting rule, closer to the real quota

### v0.2.12

- The account list shows the day's sent count and total quota

### v0.2.11

- Fixed an `ImportError` on exe startup (restored `now_beijing`)

### v0.2.10

- Fixed accounts becoming "unavailable" and blocking sending: the daily quota is now recorded in Beijing time
- A failed send no longer disables the account automatically

### v0.2.9

- Fixed skipped records not being written to the log
- Plan results now show skip / failure reasons

### v0.2.8

- Multi-account parallel sending
- Cooldown and fingerprint isolation per account

### v0.2.7

- Delivery log with paging, search, clearing and storage usage

### Earlier releases

- v0.2.6: slow mode no longer blocks the global send queue  
- v0.2.5: scheduled plans no longer auto-resume after closing the app; cooldown can be 1 second  
- v0.2.4: fixed conflicting settings saves  
- v0.2.3: group discovery with a waiting animation and success / failure feedback  
- v0.2.2: sync Telegram's official folder structure when discovering groups  

See [Releases](https://github.com/myhgjb666/telegram-group-tool/releases) for the complete archive.

---

## Responsible use

- Only send to groups you have joined and actively take part in; comply with the [Telegram Terms of Service](https://telegram.org/tos) and your local laws
- High-frequency flooding and cold outreach to strangers are not recommended
- Account sessions are encrypted and stored locally — do not copy the program folder to others

---

## FAQ

**Q: The app won't open / crashes?**  
A: Make sure you are on 64-bit Windows. Try running as administrator, or move the exe to a path without non-ASCII characters.

**Q: I don't receive the Telegram login code?**  
A: The code is normally delivered inside the Telegram app (not always by SMS). Check that the phone number is in international format (e.g. `+86...`).

**Q: Sending is very slow or messages are frequently skipped?**  
A: Check the **Status** column of the delivery log (set the filter to "All", not just sent), then the **Duration** column:  
① Many "duplicate content skipped" entries = the same static copy can only go to one group within the duplicate-content window; lower it on the **Settings** page to speed up.  
② If the copy contains variables such as `{{group_name}}` or `{{random}}`, each group gets different content and it is not treated as duplicate (`{{random}}` generates 3 letters+digits; use `{{random8}}` for a longer string).  
③ A `flood_wait Ns` entry, or a countdown on the accounts page, means Telegram is rate limiting you. Wait it out, slow the pace and vary the copy so the account is not restricted further.  
④ If **Duration** (since v0.2.25 it measures the send only, excluding pacing) shows tens of seconds or even 120+ seconds, the network / proxy is slow — a more stable route helps more than tuning parameters. The app now rotates degraded connections automatically; the status bar shows "N rebuilds".  
⑤ Other skip reasons (group cooldown, daily quota, inactive hours) are visible in the log and on the accounts page. Defaults are conservative; tune them as needed, but do not max everything out.  

**Q: Some groups can be messaged manually, but the tool never sends to them?**  
A: Check whether the row is marked blacklisted (column 5 shows "yes", or the top-right counter shows "N blacklisted"). Older versions misread Telegram's temporary restriction as a permanent ban and blacklisted those groups. Right-click the group and choose "Unblacklist", or right-click the account row and choose "Unblacklist all (N groups)" to fix them in bulk, then tick them again. Since v0.2.27 temporary restrictions are no longer blacklisted.

**Q: Sign-in says "cannot securely connect to the licensing service / certificate verification failed"?**  
A: It means the encrypted connection used for sign-in was replaced in transit — unrelated to your account. Use the issuer shown in the message: ① "certificate issued by XXX but does not match the host / is not a common public CA" → your VPN / proxy / corporate network is intercepting the connection; **switch to another node**, or add a `DIRECT` rule for the licensing host and retry. ② "certificate expired" → update to the latest build. ③ "certificate is valid but local verification failed" → usually a **wrong system clock** (right-click the taskbar clock → sync) or an outdated local root store. Seeing this right after changing VPN is almost always case ①.

For questions or bug reports, please open an [issue](https://github.com/myhgjb666/telegram-group-tool/issues) and include the version number (e.g. v0.2.28).
