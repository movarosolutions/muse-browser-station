---
name: muse-browser-station
description: Run an AI agent's web work (store patrols, social posting, back-office checks) inside the user's own browser on their own always-on computer, instead of a cloud browser — so platform sessions stay logged in on a stable home IP and risk-control challenges (SMS codes, QR scans, "verify it's you") mostly disappear. Covers station setup, Chrome profile discipline, window-binding troubleshooting, per-platform verification, a human-in-the-loop rule for high-risk platforms, a second-agent profile pattern, and cloud fallback. Use when cloud-browser automation keeps hitting login walls or verification challenges, or when setting up a home execution station for an agent.
---

# Own-Browser Execution Station

## The problem this solves

An agent working through a cloud browser looks like a stranger to every platform:
a datacenter IP, a fresh session, a device nobody has seen before. Marketplaces and
social platforms answer with email-link verifications, SMS codes, QR challenges and
occasional security alerts — the agent keeps stopping, and the user keeps rescuing it.

The fix is architectural, not cleverer automation: **move the work into the user's own
browser, on the user's own computer, on the user's home network.** The platform sees the
same device, the same IP and the same long-lived session it has always seen. Most
verification friction simply never happens.

## Architecture

```
                ┌──────────────── user's home ────────────────┐
                │  Always-on computer (desktop client paired) │
                │  ┌──────────── Chrome ────────────────┐     │
   Agent ───────┼─▶│ Work profile window                │     │
 (controls      │  │  pinned tabs: shops / social / mail  │     │
  via paired    │  ├────────────────────────────────────┤     │
  desktop       │  │ Second profile window (optional)   │     │
  client)       │  │  second agent account's web chat   │     │
                │  └────────────────────────────────────┘     │
                └─────────────────────────────────────────────┘
   Cloud browser = fallback only, when the station is asleep/offline.
```

Requirements:

- A computer that can stay at home and awake during working hours (it does **not**
  need to be on 24/7 — only across the windows when scheduled work runs; keep it
  plugged in, don't close a laptop lid, enable "keep awake" in the agent app).
- The agent's **desktop client** installed and paired on that computer. This pairing
  is what grants browser/computer control. Note the platform matrix honestly:
  if the agent product has no client for an OS (e.g. no Windows client), that OS
  **cannot** be a station — remote-desktop tools, workflow bridges (n8n), VPN
  tunnels (Tailscale) and chat bots do **not** substitute, because none of them can
  reach the browser's signed-in sessions.
- Chrome with **two profiles** (see Profile discipline).

## Profile discipline

- **Work profile** — the agent's station. All shop backends, social accounts,
  business mail and Drive live here, each as a **pinned tab**. Turn on
  "On startup: continue where you left off" so a restart restores the station.
- **Second profile (optional)** — if a second agent account joins the team, it lives
  in its own profile window, logged into its own web chat. The primary agent can
  switch to that window and talk to the second agent directly (see below).
- Never mix personal browsing into the work profile; cookie/session hygiene is the
  whole point.

## One-time setup

1. Pair the computer via the agent's desktop client; grant the OS permissions it
   asks for (accessibility / screen recording) and set control approval to
   "always allow" if you trust the arrangement, so scheduled work doesn't stall
   on per-task prompts.
2. In the work profile, sign in to each platform **once, by hand** (the human does
   all logins, codes and QR scans — the agent never types passwords).
3. Pin each platform tab; also pin webmail and Drive if the agent uses them.
4. Verify the station (next section) before moving any scheduled work to it.

## Verification checklist (per platform)

The agent opens each pinned tab read-only and confirms, from what the page itself
shows:

- [ ] The account/shop identity is the right one (name/handle on the page)
- [ ] A signed-in dashboard loads — not a login, sign-up or challenge page
- [ ] Role is as expected (e.g. seller "full access", business-profile "manager")
- [ ] Any anomaly (security alert, restriction banner) is reported, not clicked through

Record the result per platform. A platform is "on the station" only after its row
passes.

## Operating rules

- **Work only in tabs you opened, or tabs the user explicitly designated.** Don't
  wander the user's other tabs.
- **Patrols are read-only.** On any SMS-code, QR, traffic-verification or new-login
  security prompt: stop immediately, report what happened, never retry logins in a
  loop.
- **Select windows explicitly.** If two Chrome windows overlap at the same geometry,
  automatic window binding can fail (`geometry_mismatch`). List windows first, then
  activate the work window by its ID. This is the normal path, not an error path.
- **Some pages block control.** A few backends refuse to be viewed/controlled when
  opened fresh by URL, while an already-open, signed-in tab of the same backend
  reads fine. Prefer the user's pinned tabs over typing URLs.
- **Human-in-the-loop for high-risk platforms.** Platforms whose risk control scores
  *behaviour* (input rhythm, mechanical timing) — short-video / social-commerce
  apps are the strictest — are a poor fit for routine automated posting even from
  the user's own device. Pattern that works: the agent prepares drafts and assets,
  the human taps publish in the official app; agent-side web posting is an
  approved, occasional backup only. Lower-risk backends (marketplaces, microblog,
  business profiles) can run agent-side after verification.
- **Cloud is the fallback, not the enemy.** If the station is asleep or offline, a
  scheduled run may fall back to the cloud browser and must say so in its report.

## Talking to a second agent through its own window

If the second agent account is signed into its web chat in the second profile
window, the primary agent can select that window and type there — a direct
agent-to-agent channel that removes the human copy-paste relay for routine
dispatch and status chasing. Ground rules:

- The primary agent identifies itself by name/role; it never impersonates the user.
- A shared file workspace (Drive/Docs mailbox folders) remains the system of
  record; chat is the fast lane, deliverables still land as files and get verified.
- Spending and outward publishing still wait for the user's approval — neither
  agent self-approves.

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Window binding fails, `geometry_mismatch` | Two windows stacked at identical geometry | List windows, activate the target by ID |
| "App or page is blocked" on a chat page | The front tab is the agent product's own page — self-pages are excluded from control | Switch that window to a normal tab, or close it |
| Station window not listed | Window minimized or on another desktop/Space | Ask the user to restore it to the main desktop |
| Backend opens as sign-up page | That profile isn't signed in there yet | Human signs in once in that profile |
| Fresh-URL open blocked, pinned tab fine | Control layer can't verify the fresh page | Use the pinned tab; don't force it |

## Safety notes

- The agent never receives or stores passwords; all authentication is the user's,
  done by the user, in the user's browser.
- Anything the agent sends or publishes follows the team's existing approval
  rules; the station changes *where* work runs, not *who approves it*.
- Beware look-alike third-party "AI companion" browser extensions that ride on a
  known assistant's name and charge subscriptions — the station needs no paid
  extension; pairing comes from the agent product's own client.

## Origin

Distilled from a production setup running multi-platform shop patrols and daily
social posting for a small e-commerce business: six platforms verified on one home
Mac station, first agent-posted test post verified live on the work profile,
cloud browser demoted to fallback.
