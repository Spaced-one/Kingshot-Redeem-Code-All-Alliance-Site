# Kingshot Redeem Code – Batch gift code sender ✨

> Language: English (default) · Version française: [README.fr.md](./README.fr.md)

> Live site: https://spaced-one.github.io/KingshotRedeemCodeAllAllianceSite/

This repo contains a single‑page web app (`index.html`) to apply one gift code to many player IDs (e.g., an alliance) for the game Kingshot. The tool signs and sends requests like the official client and executes them one by one within server limits.

> [!NOTE]
> 100% client‑side (no backend). Requests are signed and sent as `application/x-www-form-urlencoded`.

If you found this repo out of context: this tool automates, in your browser, applying the same gift code to a list of numeric Kingshot player IDs—no server or install needed.

## Who is it for? 👥
- Alliance leads or players who want to apply a code to multiple accounts.
- Users who already have a list of IDs and one gift code to distribute.

## What it does 🔧
- Paste and clean ID lists (lines/commas/semicolons, de‑dup).
- Login for each ID (`/player`) then redeem (`/gift_code`).
- Sequential processing with cooldowns and retries to respect limits.
- Clear summary: success, already received, invalid codes, failures.
- Local lists (create/rename/delete/save/load) with player info retrieval.
- Ships with two example lists by default: “Alliance FARM” and “APX Alliance” (saved locally if none exist yet).

## Requirements ✅
- A modern browser (Chrome, Edge, Firefox, Safari).
- A valid gift code (text).
- A list of numeric player IDs.

## Quick start 🚀
1) Download/clone the repo and open `index.html` in your browser.
2) Paste your IDs (one per line or separated by commas/;).
3) Enter the gift code.
4) Click “Send request”. The script will:
   - Wait 1 second, call login (`/player`) with signature.
   - Wait another 1 second, call redeem (`/gift_code`).
   - On 429, wait 11 seconds and retry (limited attempts).
   - Stop early if the code is invalid (USED/CDK NOT FOUND).

> [!TIP]
> You can enable clipboard capture or “paste mode” to auto‑add IDs you copy.

## How it works 🧠
- Signature: sorted `key=value` pairs; MD5 with salt `mN4!pQs6JrYwV9` via CryptoJS; sent as `application/x-www-form-urlencoded`.
- Submit flow: strictly sequential per ID; 1s before login and 1s before redeem; 429 retry with a fixed 11s wait.
- Redeem statuses detected:
  - Success: `code = 0` or `err_code = 20000` (`SUCCESS`).
  - Already received: `err_code = 40008` or `msg` contains `RECEIVED`.
  - Invalid/error: `err_code ∈ {40005 (USED.), 40014 (CDK NOT FOUND.)}` → immediate stop.
  - Failure: other errors/network.
- Player info fetch: dedicated button; sequential with 1s cooldown and 429 retry; shows avatar/nickname/server/level/recharges.

> [!IMPORTANT]
> If the code is invalid/erroneous, processing stops right away to avoid useless requests on the remaining IDs.

## Troubleshooting 🧰

> [!WARNING]
> 429 Too Many Requests: the tool waits 11s then retries automatically (a few attempts). Processing is slower but avoids blocks.

> [!NOTE]
> Background tab: browsers may throttle timers. The tool adapts frequency and performs a short boost when you come back.

> [!TIP]
> Clipboard: if access is denied, use the “paste mode” (no permission).

> [!NOTE]
> CORS/network: the tool calls a third‑party domain; it depends on the server headers and your network.

## Run locally (optional) 🖥️
There’s no build. Open `index.html` directly or serve via a tiny static server.

```bash
# Python 3
python3 -m http.server 8000

# Node (via npx) — optional
# npx serve .
```

Then visit http://localhost:8000 and open `index.html`.

## Privacy and limits 🔐
- Nothing is sent to a server from this repo. Your lists/infos remain in your browser (localStorage).
- Requests go from your browser to `kingshot-giftcode.centurygame.com`.
> [!CAUTION]
> Respect the game’s terms of use. Use at your own risk.

## FAQ ❓
• Where do I get IDs?
> From your organization (alliances, exports, copies…). The tool accepts digits one per line or separated by commas/;.

• Can I change the default lists?
> Yes. Create/rename/delete your lists. They’re stored locally.

• Why is it slow?
> It’s intentionally sequential with cooldowns and 429 retries (11s) to avoid server blocks.

## Languages 🌐
The UI auto‑switches between French and English based on your browser (FR/EN). Server messages remain as returned by the API.

## License
No license provided yet.
- No build required. Just open `index.html`.