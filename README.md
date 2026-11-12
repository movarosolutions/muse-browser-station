# Own-Browser Execution Station

**Run your AI agent's web work in *your own* browser — not in the cloud.**

Cloud browsers make your agent look like a stranger: datacenter IP, fresh session,
unknown device. Marketplaces and social platforms respond with email verifications,
SMS codes, QR challenges and security alerts, and your agent spends its life asking
you to rescue it.

This skill sets up the alternative: an **execution station** on your own always-on
home computer. Your agent controls your Chrome (via its paired desktop client),
works in a dedicated work profile where every shop and social account is already
signed in and pinned, and the platforms see the same device, IP and session they
have always trusted. A cloud browser remains — as fallback only.

## What you get

- A station setup checklist (pairing, permissions, profile discipline, pinned tabs)
- A per-platform verification checklist — a platform is "on the station" only when
  its signed-in dashboard is confirmed read-only
- Operating rules that keep accounts safe: read-only patrols, stop-on-challenge,
  explicit window selection, human-in-the-loop publishing for high-risk platforms
- A two-agent pattern: a second agent account lives in its own Chrome profile
  window, and the primary agent can talk to it directly — no human copy-paste relay
- A troubleshooting table from real failures (window-binding geometry mismatches,
  self-page control blocks, blocked fresh-URL backends with healthy pinned tabs)

## Honest limits

- The station computer must be awake during your working windows (not 24/7).
- If your agent product has **no desktop client for an OS, that OS can't be a
  station** — remote desktop, n8n, VPN tunnels and chat bots cannot reach a
  browser's signed-in sessions. We learned this the hard way.
- A few platforms detect *automation behaviour*, not just IP/device. For those,
  the human still taps "publish" in the app; the agent prepares everything else.

## Install

Copy `SKILL.md` into your agent's skills directory, then ask your agent to set up
the station with you. Setup is a one-time ~20 minutes of human logins; after that
the logins simply stay.

## 中文说明

**把 AI 助手的网页工作搬进你自己的浏览器，而不是云端浏览器。**

云端浏览器在平台眼里是个陌生人：机房 IP、全新会话、没见过的设备——结果就是
没完没了的邮件验证、短信码、扫码和安全提醒。这套技能的做法是：在家里放一台
常开的电脑作为「执行站」，助手通过官方客户端控制你自己的 Chrome，在固定的
工作配置里干活——各店铺和社交账号由你本人登录一次并固定标签页，平台看到的
始终是你的设备、你的 IP、你的老会话。云端浏览器只留作兜底。

技能包包含：执行站搭建清单、双配置分工、逐平台登录验收表、只读巡检与
「遇验证即停手」规则、高风控平台（如小红书）由真人在 App 内发布的分工、
双智能体同机直连的协作模式，以及真实踩坑整理出的故障排查表。

## License

MIT
