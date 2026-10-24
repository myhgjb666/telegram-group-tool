# Telegram Bulk Message Sender — Desktop Group Broadcast Tool

[中文](README.md) ｜ **English**

A multi-account **Telegram bulk message / mass DM** desktop client for Windows. It sends to groups you have **already joined**, with message templates, scheduled plans and delivery logs — built for community announcements and routine reminders.

**Keywords:** telegram bulk message sender, telegram bulk messaging, telegram mass message tool, telegram mass DM software, telegram group broadcast tool, telegram multi-account sender, telegram auto message sender, telegram message automation, telegram marketing tool, telegram bulk sender for Windows, send bulk messages to telegram groups, telegram group messaging software, telegram broadcast desktop app, telegram session import, telegram bulk account login, telegram .session login, import telegram session file, telegram proxy support, telegram socks5 proxy, telegram one proxy per account, telegram proxy pool, rotate telegram proxies, telegram proxy checker, telegram proxy availability test, telegram keyword listener, telegram listener manual approval, telegram customer phone number import, detect telegram by phone number, telegram batch join groups, telegram auto join group, telegram bulk invite link join, TG bulk sender, Telegram 群发 / TG 群发工具 / 电报群发 / 批量加群.

> Latest version: **v0.2.43** (Windows)  
> This repository hosts the installer and usage documentation for easy download. Source code is not included.

---

## Download

| Version | File | Notes |
|---|---|---|
| **v0.2.43** (latest) | [tg-broadcast-v0.2.43.exe](https://github.com/myhgjb666/telegram-group-tool/releases/download/v0.2.43/tg-broadcast-v0.2.43.exe) | Windows 64-bit single file, run directly |
| v0.2.42 | [tg-broadcast-v0.2.42.exe](https://github.com/myhgjb666/telegram-group-tool/releases/download/v0.2.42/tg-broadcast-v0.2.42.exe) | Previous release |
| v0.2.41 | [tg-broadcast-v0.2.41.exe](https://github.com/myhgjb666/telegram-group-tool/releases/download/v0.2.41/tg-broadcast-v0.2.41.exe) | Previous release |

Older builds are available on the [Releases](https://github.com/myhgjb666/telegram-group-tool/releases) page.

**Requirements:** Windows 10 / 11 (64-bit)

If your antivirus blocks the first launch, choose "Run anyway" or add an exception — the tool is a PyInstaller-packed desktop app and is prone to false positives.

---

## Features

- **Multi-account management** — add several Telegram accounts, check status, daily sent count and daily quota
- **Proxy pool (one proxy per account / shared proxies)** — import http / https / socks5 proxies in bulk, auto-assign one line per account or let several accounts share one; the account list shows which line each account is on, and the built-in test performs a **real proxy handshake** into Telegram instead of only checking whether the port answers
- **Bulk `.session` login** — import already-authorized `.session` files directly, with **no login code and no 2FA prompt**; pick several files at once or point it at a whole folder, and it verifies each one and lists success / skipped / failed; if the same folder holds a sidecar json (`16803619008.json` etc.) the `api_id` / `api_hash` and device info are read from it automatically
- **Group picker** — sync your joined groups and tick the ones you want to send to
- **Join tasks (bulk group joining)** — paste a batch of public group links / private invite links, pick one account and let it join at a conservative pace (about a dozen per account per day by default); each line can carry an "expected owner" that is cross-checked after joining and only flagged, never enforced; joined groups land on the Groups page ready to be used as send targets
- **Message templates** — multiple variants with rotation and variables (date, time, group name, random string)
- **Scheduled plans** — cron or fixed interval, run now / pause anytime
- **Anti-ban pacing** — configurable cooldown, daily quota, daytime window and jitter
- **Delivery log** — success / failure reasons, per-message duration, searchable and paginated
- **Keyword listener** — auto-reply to group messages or private chats when a keyword matches, with cooldown and daily cap
- **Listener hits (manual confirm)** — switch a rule to "record only": matches land on the new **Listener hits** page, you tick the people to contact, pick a template and confirm, and only then are the messages sent
- **Public group search** — search public groups / channels by keyword, join at a limited rate and add them as targets
- **Customer outreach** — import your own customers' phone numbers, detect which of them have Telegram, and send one-to-one follow-ups (own customers only, with permanent opt-out and daily caps); detected numbers can be cleared from your Telegram contacts afterwards; import picks a country so bare numbers get the right prefix automatically (30+ countries/regions incl. mainland China, HK/Macao/Taiwan, Southeast Asia, Europe & US), and numbers support multi-select batch delete plus one-click clear-all
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

1. Download and run `tg-broadcast-v0.2.43.exe`
2. Sign up / sign in to the app account (new devices usually get a trial period, then a license key is needed)
3. Open [my.telegram.org/apps](https://my.telegram.org/apps), create an application and copy your `api_id` / `api_hash`
4. On the **Accounts** page, add a Telegram account (API credentials + phone number, then the login code / 2FA password)
   - If you already have authorized **`.session`** files, use "Import .session…" to skip the login code entirely — see the next section
5. On the **Groups** page, discover your groups and tick the targets
6. Create content and a plan under **Templates** / **Schedules**, then start sending

---

## Bulk login with `.session` files

If you already have a batch of authorized accounts (`.session` files), you do not have to walk each one through the SMS code. On the **Accounts** page, click **"Import .session…"**.

### Steps

1. **Pick files** — "Choose files…" accepts multi-select; "Choose folder…" imports a whole directory (tick "include subfolders" to scan nested ones). Each file is parsed locally right away and its row shows the result, e.g. `Telethon · DC 2`; broken files are marked red before any network call is made.
2. **API credentials: usually read for you** — since v0.2.34 the app first looks for a **sidecar json in the same folder** (e.g. `16803619008.json`, `session.json`; sellers usually ship it next to the `.session`). If it contains `app_id` / `app_hash` (or `api_id` / `api_hash`), those are filled in and the row is marked "自带 api (xxx.json)". You only type a pair when no such file exists; tick "Remember" and later imports need nothing but the file. Session files themselves never contain the `api_hash`, so with neither source this step cannot be skipped.
3. **Click "Import and verify"** — the app connects to Telegram and reads the account info before writing anything to the database. Rows turn **green (success) / amber (skipped — already in the list) / red (failed)**; hover for the full reason.

### Supported formats

| Format | Supported | Notes |
|---|---|---|
| Telethon `.session` | ✅ | The most common one |
| Pyrogram `.session` | ✅ | Has no server address; filled in from the DC table |
| Plain-text StringSession | ✅ | `.session.txt` / `.txt` — some sellers hand out text |
| Telegram Desktop `tdata` folder | ❌ | A completely different structure, not the same kind of session file |

**Your original files are never modified** — the app reads only the auth key, works on a temporary copy and deletes it immediately, so bytes and mtime stay untouched.

### Batch pacing

- Runs sequentially, pausing **1–2 seconds between files** (with jitter) so dozens of accounts do not come online in the same second.
- A single failure only records one row and **continues with the next file**, so one broken account cannot kill the batch.
- The batch only aborts early when the API credentials themselves are wrong — otherwise the same error would repeat dozens of times.
- You can press "Stop" at any time; the window does not close automatically when the run ends, so you can see which files failed.

### Three limits you should know

1. **A `.session` file is full control of that account.** If someone else handed you the account, they still hold the same key and can sign back in and kick you offline. After importing, open **Settings → Devices** on your phone and terminate any session you do not recognize.
2. **The device shown for that session will not change.** It reflects whatever device originally authorized it (a phone, or some library). Changing device info locally only affects authorizations created afterwards.
3. **The 2FA password is not stored in the file.** It is not needed while the session is alive, but if the session dies and you must sign in again, only you know that password.

Imported accounts are **identical** to manually added ones: reconnect, broadcasts, scheduled plans, keyword listening, customer outreach and daily quotas all work the same way.

---

## Proxy pool — one proxy per account or shared

Previously every account had to share a single global proxy, so several accounts coming online from the same IP was easy to spot. From v0.2.33 there is a **Proxy pool** page: import a batch of proxies and either give each account its own line, or let a few accounts share one.

### Steps

1. **Import proxies** — open the **Proxy pool** tab, paste a list or use "Import from file". Two notations are accepted and can be mixed:
   - Full URL: `socks5://user:pass@1.2.3.4:1080`, `http://user:pass@1.2.3.4:8080`
   - Colon form: `1.2.3.4:1080:user:pass` (port required, credentials optional)
2. **Test the batch** — click "Test all". Each proxy goes through a **real handshake**: SOCKS5 greeting (with credentials when required) → ask the proxy to open a tunnel to Telegram's servers. Only a successful tunnel counts as "available". Checking the port alone is not enough: plenty of lines keep port 443 open (a CDN sits in front) while the proxy itself is long dead, and the handshake is dropped immediately.
3. **Assign** — click "Auto assign" and the app hands one proxy to every account that has **no binding yet** (already bound accounts are left alone). You can also pick a proxy per account in the account editor. The binding is saved immediately and reconnecting happens separately, so a failed reconnect never changes what was saved.
4. **Check** — the account list has a new **Proxy** column showing the line each account currently uses; the proxy list shows how many accounts use each proxy (more than 1 means it is shared).

### Supported protocols

| Protocol | Notation | Notes |
|---|---|---|
| HTTP | `http://...` | Common low-cost proxies |
| HTTPS | `https://...` | Handled as an HTTP tunnel |
| SOCKS5 | `socks5://...` | Recommended for Telegram, supports username / password |

### How to read "available / unavailable"

The **Status** column has exactly three values:

| Status | Meaning |
|---|---|
| Untested | Just imported, or the verdict was produced by an older build and has been invalidated |
| Available | The handshake succeeded — this proxy really carries traffic to Telegram |
| Unavailable | Hover for the reason: port alive but proxy dead / IP allow-list / bad credentials / proxy refuses Telegram / HTTP 407 … |

The most common one you will see is "**the port answers, but the proxy dropped the handshake**" — that line is dead, replace it instead of tuning settings.

- **Test before assigning**: run "Test all" first (6 concurrent, ~20 s worst case per proxy), delete or replace the unavailable ones, then assign.
- **After switching to a new batch**: bindings still point at the old proxies — assign again (or bind manually). If you only want the existing bindings reconnected, press "Reconnect bound accounts".
- The global proxy test in Settings uses the same logic, so the verdicts agree.

### Things to know

- **Takes effect immediately** — changing an account's proxy reconnects that account automatically; no need to restart the app.
- **A broken binding never falls back to a direct connection** — if a proxy is deleted, broken or misconfigured, the account falls back to the **global proxy** instead of connecting bare and exposing your real IP.
- **Credentials are stored encrypted** — proxy usernames and passwords are encrypted locally and never shown in plain text; importing the same proxy (address + credentials) twice is de-duplicated.
- **The global proxy still works** — accounts without a dedicated proxy keep using the global proxy from Settings.

---

## Join tasks — import links in bulk, join at a humane pace

When you have a batch of group links (from buying groups or taking over a community), joining them used to mean clicking through one by one. Since v0.2.43 there is a **Join tasks** page: paste the links in bulk, pick one account, and it joins at a conservative pace; joined groups land on the Groups page automatically.

### Steps

1. **Import links** — paste into the text box, one per line. These forms are accepted (optionally followed by `| @expected_owner` to note the source):

   | Notation | Type |
   |---|---|
   | `https://t.me/abcd1234` / `t.me/abcd1234` | Public group |
   | `t.me/+AbCdEf123` / `t.me/joinchat/AbCdEf123` | Private invite link |
   | `@abcd1234` or bare `abcd1234` | Public group |
   | `tg://resolve?domain=abcd1234` / `tg://join?invite=AbCdEf123` | Deep link |

   Click "Parse and add" to validate first, or "Import from file…"; duplicate links within one batch are kept once.
2. **Pick an account** — choose a signed-in account from the dropdown. Whichever proxy that account is bound to is the exit the joins go through (one-proxy-per-account works as usual).
3. **Click "Start joining"** — the app joins one link at a time, waiting a few random minutes in between. "Stop" exits after the current link finishes; remaining tasks stay pending for the next run.

### What the expected owner is for

When buying groups or taking one over, put the username the seller gave you in this column. After a successful join the app looks up the actual owner into the "seen owner" column: a match shows normally; a mismatch shows **⚠️ owner mismatch** — **recorded only, never enforced, no auto-leave** (admins change all the time), leaving is your call. If the group hides its member list, the check is left blank.

### Status meanings

| Status | Meaning | Retried? |
|---|---|---|
| Pending | Not reached yet | Handled by the next "Start joining" |
| Joined / already in group | Done | No |
| Approval pending | Such groups require an admin's approval first; you become a member once approved | Nothing to do |
| Dead link | Invite expired / group dissolved / no access | **Never retried** (a retry would only burn the daily quota) — ask the seller for a fresh link |
| Failed (retryable) | Temporary network / rate-limit errors | Retried automatically next run, or use "Retry failed" to requeue manually |

### Pacing (conservative by design)

- **Daily cap**: 12 joins per account per day by default, rolling over at midnight Beijing time; only "joined / already in group / approval pending" consume the quota
- **Per-link interval**: 5–15 random minutes by default, adjustable in **Settings → Join pacing** (floor 5 seconds)
- **Active hours**: reuses the same daytime window as the send engine
- **Stops immediately on limits**: a Telegram FloodWait or hitting the ~500-channels-per-account cap ends the run right away and keeps the remaining tasks, instead of hammering

> **Scope**: join groups you have a genuine reason to be in (your communities, customer groups, invited groups). Mass-joining unrelated stranger groups is what purchased accounts do — bans come faster there than from sending.

---

## Changelog

### v0.2.43 (current)

- **New: Join tasks** — paste a batch of group links (public `t.me/xxx`, private invite `t.me/+xxx` / `joinchat`, deep links), pick one account and let it join at a conservative pace; joined groups land on the Groups page with "send target" pre-ticked
- **Expected-owner verification** — each line may carry `| @owner_username`; after joining the actual owner is looked up and compared, mismatches are flagged ⚠️ only, never enforced
- **Three pacing gates** — daily cap of 12 per account per day by default (midnight Beijing rollover), 5–15 random minutes between links, active-hours window shared with the send engine; a Telegram FloodWait or the channel cap stops the run immediately, keeping remaining tasks
- **Dead links are never retried** — expired invites / dissolved groups are marked dead and excluded from retries so they don't burn the daily quota; network failures retry automatically
- Full steps, status table and pacing notes: see [Join tasks](#join-tasks--import-links-in-bulk-join-at-a-humane-pace)

### v0.2.42

- **One-by-one "detect then send"**: sending no longer batch-detects the whole list first — when the loop reaches a number it **detects that single number**, messages it immediately if found on Telegram, then waits out the send interval before the next one. Two consecutive lookups are naturally a full send interval apart — the gentlest possible pace against Telegram's rate limits
- **Auto-pause on rate limit**: the task pauses; remaining numbers stay "undetected". Click "Continue sending" later and it resumes with detect-then-send for the undetected ones
- **Live status line**: "Detecting 138…" / "Sending … (N/M)"; after the first 3 items it extrapolates "estimated to finish around HH:MM"
- **Freer tick boxes**: undetected/failed numbers can now be ticked too (opted-out still cannot); by default only already-detected sendable ones are ticked
- Numbers judged "not registered / privacy-restricted" are not re-detected repeatedly (no wasted rate-limit budget on dead numbers); failed detections retry automatically on the next run

### v0.2.41

- First version with auto-detection before sending and estimated finish time (v0.2.42 improved "batch-detect first" into **one-by-one detect-then-send**, much friendlier to rate limits on big lists — prefer v0.2.42)

### v0.2.40

- **Removed the per-account daily detection cap**: previously each account could detect at most 2000 numbers per day and then showed "daily quota exhausted". There is no cap anymore — detection now runs until the list is done or Telegram rate-limiting pauses it
- The "today's remaining quota" hint now reads "no daily limit"; usage is still recorded and visible in the app
- The 1000-numbers-per-import limit, detection pacing, and the auto pause/resume on rate limits are unchanged — removing the app-side cap doesn't remove Telegram's own rate limits, so large lists are still best split across accounts

### v0.2.39

- **Fixed: false "no Telegram / privacy-restricted" results appearing in batches**. Telegram has a second rate-limit shape for phone-number lookups: **no error, but results are silently suppressed** (the response comes back empty). The app used to mark every number in such a chunk as "not registered", and that status is never retried — a large list could be burned into false "not registered" wholesale. "20 numbers work fine, more all show no-TG" is exactly this
- **Auto-recovery**: when a chunk comes back with zero hits, the app now immediately checks the account's address book — suppressed imports have usually already written the numbers into it, so they can be matched by phone number and correctly marked "has Telegram"
- **Auto-pause**: if several consecutive chunks (including the address-book check) still yield nothing, the app treats it as rate limiting and pauses: misjudged numbers are reclassified as "failed" (auto-retried on the next run), remaining numbers stay "undetected" and won't be polluted further. The status line explains the pause reason
- **Gentler pacing**: inter-chunk delay raised from 3–5 s to 6–12 s to reduce the chance of triggering the limit
- **Upgrade note**: numbers misjudged as "no TG / privacy-restricted" on v0.2.38 or earlier won't recover automatically (that status isn't retried). Delete and re-import those numbers, or just carry on — numbers from suppressed imports are usually already in the account's address book, so re-import + detect recovers them from the address book at no extra detection cost

### v0.2.38

- **Fixed: detection of large lists (hundreds–thousands of numbers) stalled halfway**. Telegram rate-limits phone-number lookups (importContacts) hard — with a big list (20 chunks fired back to back) a FloodWait is guaranteed mid-run. Previously the app kept hammering the remaining chunks, which only stretched the ban longer, showing up as "detection freezes halfway". The app now **pauses immediately on FloodWait**; already-processed numbers keep their results
- **Resumable**: paused numbers stay "undetected"; after the cooldown (the status line shows the approximate wait), clicking "Start detection" again resumes from where it stopped — already-detected numbers are not re-detected
- **No quota wasted**: quota is now charged per actually-processed number; numbers skipped by the pause don't consume today's detection quota
- Large lists are best split across days and accounts; the per-account daily cap is unchanged

### v0.2.37

- **Fixed: Customer outreach "Start detection" completed but every status stayed "未识别" (undetected)**. Detection itself ran correctly (account selection, quota, rate limiting all fine), but the step that writes results back to the database had an internal error — every result was silently discarded. Detected "has Telegram" customers now appear in the table as expected and can be ticked for sending
- **Upgrade note**: for numbers detected on v0.2.36 or earlier, run "Start detection" once after upgrading — previously detected numbers are re-checked without extra quota, and failed ones retry automatically
- No other changes

### v0.2.36

- **Fixed: the country picker was ignored on import**. In the previous release the legacy "11 digits starting with 1 = mainland China mobile" rule outranked the picker — with "USA/Canada +1" selected, `18607123456` (= 1 + 860…, a US number written with its country code) was silently rewritten to `+8618607...`. The **explicit country choice now always wins**
- **Single-digit codes (+1 / +7) handled properly**: input shaped like `<code><10 digits>` starting with the code (e.g. 18607123456) is treated as already carrying the country code instead of getting a doubled 1; bare 10-digit numbers (e.g. 2125551234) get `+1` prepended as usual
- With no country selected the behaviour is unchanged: 11 digits starting with 1 still gets +86
- If you imported non-+86 numbers on v0.2.35, upgrade, remove the ones that were rewritten to +86, and re-import them

### v0.2.35

- **Country picker on import** — the Customer outreach import form now has a **Country** dropdown with 30+ common countries/regions (mainland China by default, HK/Macao/Taiwan, Southeast Asia, Japan/Korea, Europe & US). Numbers pasted without a `+` prefix get the selected code prepended automatically; numbers that already carry `+` are used as-is
- **Batch delete & clear all** — the number list supports Ctrl / Shift multi-select, and "Remove selected" deletes the whole selection at once; a new "Clear all" button (with double confirmation) wipes the entire list in one go
- **Deletion only touches number records** — both deletion paths keep your delivery logs; "Clear all" also removes opt-out records (stated clearly in the dialog), so re-imported numbers can be contacted again — use with care
- **Detection flow unchanged** — import first, then detect, then send, exactly as before; this release does not touch detection pacing or quotas

### v0.2.34

- **Proxy testing now performs a real handshake** — previously it only checked whether the port answered, so a line with a live 443 port but a dead proxy service showed as "reachable" and every assignment failed afterwards. Now the app speaks the actual protocol (SOCKS5 with credentials, SOCKS4, HTTP CONNECT) and asks the proxy to tunnel **to Telegram's servers**; only that counts as "available"
- **Clearer status** — `Untested / Available / Unavailable`, with the reason on the Status cell (hover): e.g. "the port answers but the proxy dropped the handshake (port alive, service dead, or IP allow-list required)"
- **Faster and honest** — 6 concurrent tests, dozens of proxies in seconds; verdicts from the old logic are invalidated on upgrade so stale green marks cannot mislead you
- **Fixed "assignment failed"** — assignment now only writes the binding and reports immediately; reconnecting is a separate stage (20 s per account, 4 concurrent) that reports "reconnected x / failed y" without ever contradicting the saved binding. A proxy failure no longer marks a healthy account as banned, and a new "Reconnect bound accounts" button retries in one click
- **Listener: manual confirmation** — a rule can be set to "manual": matches are only recorded, never sent. On the new **Listener hits** page you tick the people to contact, pick a template and confirm (it reuses the DM pacing and daily quota and shows head-count and estimated duration before sending)
- **`.session` import reads API credentials automatically** — a sidecar `xxx.json` in the same folder (`app_id` / `app_hash` or `api_id` / `api_hash`) is used to fill them in; batch imports use each account's own credentials and mark rows with "自带 api"
- **More robust startup** — the tab area is disabled until initialization finishes (no more "tab used before bind()"), and a failing page no longer breaks the others
- **Consistent data across pages** — tabs refresh when you switch to them, account changes are broadcast everywhere, and deleting an account lists its related data (proxy binding, schedules, listener rules) instead of silently cascading

### v0.2.33

- **New proxy pool** — a **Proxy pool** tab lets you import http / https / socks5 proxies in bulk, accepting both full URLs (`socks5://user:pass@1.2.3.4:1080`) and the colon form (`1.2.3.4:1080:user:pass`), or importing a whole file at once
- **One proxy per account** — "Auto assign" gives every unbound account its own line in order, so accounts stop piling onto the same exit IP; accounts that already have a binding are never reshuffled
- **Shared proxies supported too** — assign any proxy to as many accounts as you like; the list shows "used by N accounts" so you can see which lines are shared
- **New Proxy column in the account list** — see at a glance which line each account is on
- **Reconnects automatically** — changing an account's proxy drops and re-establishes that connection; no app restart needed
- **Never falls back to a direct connection** — if a bound proxy is deleted, broken or misconfigured, the account falls back to the global proxy rather than exposing your real IP
- **Encrypted, de-duplicated credentials** — proxy credentials are encrypted at rest and never displayed; the same address + credentials imported twice is de-duplicated, and comment lines are not counted as failures
- Full steps, protocol table and caveats: [Proxy pool — one proxy per account or shared](#proxy-pool--one-proxy-per-account-or-shared)

### v0.2.32

- **Bulk `.session` login** — a new "Import .session…" action on the Accounts page brings already-authorized `.session` files straight in, with **no SMS code and no 2FA prompt**
- **Import a whole batch at once** — multi-select files, or point it at a directory (optionally including subfolders). `-wal / -shm / -journal` sidecars, hidden files and non-session files are skipped automatically
- **Parsed locally before anything is sent** — each row shows format and DC, so a broken file is flagged before it costs a network round-trip
- **Colour-coded results** — green success, amber skipped (account already in the list), red failure; hover for the full reason, and the window stays open when a batch finishes
- **Read-only** — the auth key is read from a temporary copy that is deleted immediately; original files keep their bytes and mtime
- **Three formats supported** — Telethon `.session`, Pyrogram `.session` (server address filled from the DC table) and plain-text StringSession; Telegram Desktop `tdata` folders are not supported
- **One failure does not sink the batch** — it records one row and moves on; only wrong API credentials abort the run; you can stop at any time
- **Imported accounts behave exactly like manually added ones** — reconnect, broadcasts, schedules, keyword listening, customer outreach and quotas are all shared
- Full steps, format table and caveats: [Bulk login with `.session` files](#bulk-login-with-session-files)

### v0.2.31

- **Device info is now detected from your real system** — the device name and OS version reported when signing in to Telegram are no longer a single hard-coded app name; they are detected from the machine you run on (Windows / macOS report their own values). Settings → "Device info" shows exactly what is reported
- Previously every account showed the same device name in Telegram's *Active Sessions*, which made the third-party client obvious and added a correlation link between accounts on one machine. Now it looks like an ordinary computer
- Already signed-in accounts stay logged in; the new device name shows from the next connection

### v0.2.30

- **New "Remove from TG contacts" action in the Customer outreach tab** — detecting numbers adds them to your Telegram account's contact list (that is how Telegram's lookup works and it cannot be avoided). You can now clear the detected customers out of the address book in one click, keeping the account tidy
- Cleanup is submitted in batches so Telegram does not reject it outright; when the request fails the app says "retry later" instead of pretending the address book is clean. The status line and the list show how many are still pending
- **Note**: cleanup invalidates that customer's Telegram addressing info, so re-detection is needed before you can DM them again. Clean up after your messages have gone out
- Your customer records, delivery logs and opt-out records are all unaffected — customers who opted out stay permanently unsent
- Fixed an internal bug: the cleanup call read the wrong field from Telegram's reply, so it always reported "0 removed"

### v0.2.29

- **New "Customer outreach" tab** — follow up with customers from your own contact base. Paste or import phone numbers (supports `number,note`, auto-deduplicated) → pick an account to detect which numbers have Telegram → send follow-up messages one by one
- **Three detection outcomes** — has Telegram (contactable), not registered or the user hides their number from being found (follow up by phone instead), or the lookup itself failed (retryable)
- **A reply of "opt out" stops everything permanently** — if the customer replies with "退订 / T / TD / UNSUBSCRIBE", the lead is permanently marked, a single confirmation is sent back, and no task will ever message them again. Opt-out takes priority over every other rule
- **Hard caps to keep it from drifting** — at most 1000 numbers per import, and no export of detection results; direct messages consume the *same* daily quota as group broadcasts; DM tasks are one-shot with no recurring schedule option
- **Fixed duplicate execution of monitor rules** — repeatedly editing a monitor rule used to leave the old handler registered, so a single incoming message could be processed several times (duplicate auto-replies, inflated match counts). Saving a rule now correctly replaces the old one
- **Scope** — this feature is for your own customers, i.e. people who left their number expecting to be contacted. Do not use it on numbers collected elsewhere; that is cold outreach, which risks bans and may violate personal-data protection laws
- Note: detecting numbers adds them to your Telegram account's contact list (that is how Telegram's lookup works and it cannot be avoided). You can delete those contacts in the Telegram app afterwards

### v0.2.28

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
- An imported `.session` file is full control of that account: only import accounts that are yours. After importing a session someone gave you, terminate the other sessions under **Settings → Devices** on your phone

---

## FAQ

**Q: The app won't open / crashes?**  
A: Make sure you are on 64-bit Windows. Try running as administrator, or move the exe to a path without non-ASCII characters.

**Q: I don't receive the Telegram login code?**  
A: The code is normally delivered inside the Telegram app (not always by SMS). Check that the phone number is in international format (e.g. `+86...`).

**Q: I already have authorized `.session` files — do I still need the login code or the API credentials?**  
A: Neither, in most cases. On the Accounts page click "Import .session…", pick the files (multi-select, or a whole folder). If the folder holds a sidecar `xxx.json` with the app credentials, `api_id` / `api_hash` are filled in automatically and the row is marked "自带 api". Then "Import and verify" — no code, no 2FA prompt, and your original files are not modified. You only type a pair manually when no such json exists. See [Bulk login with `.session` files](#bulk-login-with-session-files).

**Q: Can the keyword listener hold replies for my approval instead of sending automatically?**  
A: Yes. Set the rule's **Response mode** to **manual**: a match is only recorded, nothing is sent. Then open the **Listener hits** page, filter by rule / status, tick the people you want to contact, choose a message template and confirm — only then are the messages sent. Repeat hits from the same user are merged into one row (with a hit count), and the confirmation shows the head-count and estimated duration; you can stop mid-run.

**Q: The proxy shows "available", but accounts still fail to connect?**  
A: Since v0.2.34 "available" means the proxy completed a handshake and tunnelled to Telegram — much stricter than the old "port reachable". If it still fails: ① press "Reconnect bound accounts" and read the reason on the accounts page (status / note column); ② make sure the proxy allows your current exit IP (some require an allow-list); ③ IP-whitelisted proxies die as soon as your network changes; ④ one proxy shared by many accounts is easy to rate limit — use one line per account. If the status says "unavailable", hover it: the tooltip tells you which step failed (port alive but dead / auth rejected / refuses Telegram / 407).

**Q: The import says "auth key unregistered / session revoked"?**  
A: That session is dead — the account was logged out, the password changed, or the same file is in use somewhere else (Telegram invalidates the earlier session). Such a file can only be fixed by signing in with the SMS flow again. Importing the same account twice is unnecessary: the app tells you it is already in the list.

**Q: Sending is very slow or messages are frequently skipped?**  
A: Check the **Status** column of the delivery log (set the filter to "All", not just sent), then the **Duration** column:  
① Many "duplicate content skipped" entries = the same static copy can only go to one group within the duplicate-content window; lower it on the **Settings** page to speed up.  
② If the copy contains variables such as `{{group_name}}` or `{{random}}`, each group gets different content and it is not treated as duplicate (`{{random}}` generates 3 letters+digits; use `{{random8}}` for a longer string).  
③ A `flood_wait Ns` entry, or a countdown on the accounts page, means Telegram is rate limiting you. Wait it out, slow the pace and vary the copy so the account is not restricted further.  
④ If **Duration** (since v0.2.25 it measures the send only, excluding pacing) shows tens of seconds or even 120+ seconds, the network / proxy is slow — a more stable route helps more than tuning parameters. The app now rotates degraded connections automatically; the status bar shows "N rebuilds".  
⑤ Other skip reasons (group cooldown, daily quota, inactive hours) are visible in the log and on the accounts page. Defaults are conservative; tune them as needed, but do not max everything out.  

**Q: In Join tasks, what is the difference between "dead link" and "failed (retryable)"?**  
A: "Dead link" = the invite expired, the group was dissolved, or you have no access — a retry can never succeed, so the app marks it dead and skips it forever (so it doesn't burn your daily join quota). Ask the seller for a fresh link and import that. "Failed (retryable)" = temporary trouble such as a network blip or rate limiting; the next "Start joining" retries it automatically, and "Retry failed" requeues it manually.

**Q: Join tasks stopped after just a few joins — why?**  
A: One of the four gates tripped; the status bar tells you which: ① daily quota used up (12 per account per day by default, midnight Beijing rollover) → run again tomorrow or raise it in **Settings → Join pacing**; ② outside the active hours → wait for the window; ③ Telegram rate limit (FloodWait) → wait out the countdown and click again; ④ the account hit its ~500 channel cap → leave some groups first. Manual "Stop" also halts after the current link, with all remaining tasks kept in the list.

**Q: My customer numbers have no +86 prefix — can I import them directly?**  
A: Yes. Pick the **Country** on the Customer outreach import form (mainland China by default) and bare numbers get that code prepended automatically; numbers that already start with `+` are kept as-is regardless of the country choice. If something goes wrong in the list, hold Ctrl / Shift to multi-select and remove them in bulk, or use "Clear all" to start over (it asks twice; delivery logs are never touched).

**Q: Detection shows "paused: Telegram rate limit" — what should I do?**  
A: Telegram rate-limits phone-number lookups in two ways: an explicit FloodWait (shows the wait in seconds) and silent suppression (no error but empty results — since v0.2.39 the app cross-checks the address book to recover numbers and pauses after consecutive empty chunks). Both pause automatically with the reason shown; the remaining numbers stay "undetected" — wait a bit, then click "Start detection" again and it resumes where it stopped. Already-detected numbers are never re-detected and don't consume quota twice. Split large lists across days and accounts.

**Q: Some groups can be messaged manually, but the tool never sends to them?**  
A: Check whether the row is marked blacklisted (column 5 shows "yes", or the top-right counter shows "N blacklisted"). Older versions misread Telegram's temporary restriction as a permanent ban and blacklisted those groups. Right-click the group and choose "Unblacklist", or right-click the account row and choose "Unblacklist all (N groups)" to fix them in bulk, then tick them again. Since v0.2.27 temporary restrictions are no longer blacklisted.

**Q: Can I give each account its own proxy?**  
A: Yes. Open the **Proxy pool** tab and import your proxies (`socks5://user:pass@ip:port`, `http://...` and `ip:port:user:pass` all work, and you can import from a file), then click "Auto assign" — the app hands one proxy to every account that has no binding yet. Sharing one proxy across several accounts works too; the list shows how many accounts use each line. Bound accounts reconnect automatically and the account list shows the current proxy.

**Q: If a proxy dies or I delete it, will the account connect directly and expose my real IP?**  
A: No. When a bound proxy is broken or deleted, the account falls back to the global proxy configured in Settings. It only connects directly when no global proxy is configured either — the same behaviour as before.

---

**Q: Sign-in says "cannot securely connect to the licensing service / certificate verification failed"?**  
A: It means the encrypted connection used for sign-in was replaced in transit — unrelated to your account. Use the issuer shown in the message: ① "certificate issued by XXX but does not match the host / is not a common public CA" → your VPN / proxy / corporate network is intercepting the connection; **switch to another node**, or add a `DIRECT` rule for the licensing host and retry. ② "certificate expired" → update to the latest build. ③ "certificate is valid but local verification failed" → usually a **wrong system clock** (right-click the taskbar clock → sync) or an outdated local root store. Seeing this right after changing VPN is almost always case ①.

For questions or bug reports, please open an [issue](https://github.com/myhgjb666/telegram-group-tool/issues) and include the version number (e.g. v0.2.43).
