# best vps for openclaw: The RAM You Actually Need, Renewal Pricing That Doesn't Jump, and Where RackNerd Fits

Most people searching this want one answer: how big a server do I need, and what should it cost. The short version — OpenClaw runs fine on a **2 vCPU / 4 GB RAM VPS with 40 GB+ SSD**, it does **not** need a GPU because model calls go to a remote API, and the official pre-built Docker image means you can skip compiling anything. The part most comparison articles gloss over: what you'll pay in year two and three, not just year one.

RackNerd is worth a serious look here for one reason that most "best VPS" lists never mention: its special pricing renews at the same rate for as long you keep the service, so the $59.99/year you pay today is what you'll pay every year. That changes the math quite a bit when you compare it against intro-then-double pricing elsewhere.

> **Quick picks**
> - Running OpenClaw via Docker yourself (the common route): the **4 GB KVM special at $59.99/year** (~$5.00/month equivalent) is the comfortable choice — 3 vCPU, 60 GB SSD, 7 TB transfer.
> - Just a Telegram/Discord bot: the **2 GB plan at $35.99/year** handles it.
> - Want RackNerd's one-click OpenClaw installer instead of manual Docker setup: their own docs size that at **8 GB RAM**, so the $119.99/year plan.
>
> 👉 [Check the RackNerd 4 GB KVM special at $59.99/year](https://my.racknerd.com/aff.php?aff=11397&pid=954)

## How much server does OpenClaw actually need?

Less than the AI branding suggests. OpenClaw sends model inference to a remote provider, so the VPS only runs the agent itself — no GPU required, and any plain CPU VPS will do. A well-known community breakdown of 19 production containers (August 2026) found median resident memory of just **374 MB**, with a 90th percentile around 1,265 MB and a max of 1,411 MB. The typical agent is small; the tail is roughly three times the median, which is why sizing for the tail means 4 GB, not 1 GB.

A few practical numbers worth knowing before you pick a plan:

- **Don't build from source on a small box.** Some guides claim OpenClaw "needs 6 GB of RAM" — that figure is for compiling the image locally. A memory-starved build gets killed with exit code 137. Pulling the pre-built image from `ghcr.io/openclaw/openclaw` or Docker Hub avoids the whole problem, which is why 2–4 GB is enough for running it.
- **Budget disk for images, not just data.** The default image (2026.9.8) is about 1.2 GB compressed; the `-browser` variant with Chromium baked in is about 1.7 GB. You'll want headroom for a couple of versions as you update.
- **Plugin installs cause short spikes.** Channels and skills not bundled in the image get fetched from npm at install time — a brief hit of CPU, RAM and disk. A 1 GB box leaning on swap gets slow here.
- **Arm64 works** for the official images, including the browser variant. The exception: some skills download helper binaries, so check for a `linux-arm64` release before depending on one.
- Node version is handled by the installer's own check; keep a recent release and you're fine.

Chat history and workspace files grow slowly, so disk rarely becomes the constraint. SSD-backed storage for state and cache paths matters more — the official Linux server guide specifically recommends it to avoid slow cold starts.

## RackNerd's full KVM VPS special lineup

RackNerd currently lists five annual KVM specials, all including 1 IPv4 address, a 1Gbps port, full root access, and the ctrl control panel. Prices below are the current published rates — and per RackNerd's own specials page, they're "lifetime recurring": you renew at the same rate for as long as you keep the service.

| Plan | vCPU / RAM / SSD | Monthly transfer | Price | Order |
| --- | --- | --- | --- | --- |
| 1 GB KVM (most popular) | 1 / 1 GB / 20 GB | 3 TB | **$21.99/yr** | [Order the 1 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=952) |
| 2 GB KVM | 2 / 2 GB / 35 GB | 5 TB | **$35.99/yr** | [Order the 2 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=953) |
| 4 GB KVM | 3 / 4 GB / 60 GB | 7 TB | **$59.99/yr** | [Order the 4 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=954) |
| 6 GB KVM | 6 / 6 GB / 100 GB | 12 TB | **$89.99/yr** | [Order the 6 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=955) |
| 8 GB KVM | 7 / 8 GB / 150 GB | 20 TB | **$119.99/yr** | [Order the 8 GB plan](https://my.racknerd.com/aff.php?aff=11397&pid=956) |

Every plan supports CentOS, Rocky Linux, AlmaLinux, Fedora, Debian, or Ubuntu, with custom ISO installs available through a support ticket. Deployment happens across roughly 20 datacenter locations, and the ctrl panel gives you reboot, OS reinstall, an HTML5 VNC console for recovery, a network firewall for inbound rules, and root password resets — the things you'd otherwise need a separate panel for.

## One-click install vs. Docker: pick your RAM tier accordingly

Here's a wrinkle that most OpenClaw hosting articles don't cover, because most of them aren't looking at RackNerd: the official one-click app listing in the ctrl panel sizes OpenClaw at **at least 8 GB of RAM**. That's RackNerd's own guidance for its automated installer, alongside 4 GB minimums for Hermes Agent, AnythingLLM, Ollama, and LibreChat, and 2 GB for n8n or Coolify.

The community consensus, meanwhile, is that 2 GB runs a chat bot and 4 GB runs a production workflow comfortably — using the pre-built image. Both can be true: one-click installers provision for the worst case, and manual Docker deployment lets you run lean.

So the decision rule is simple:

- Want the ctrl one-click flow, where you pick OpenClaw from a menu and credentials show up in-panel when it's ready → the **8 GB plan at $119.99/year**.
- Comfortable running `docker pull` and a compose file over SSH → the **4 GB plan at $59.99/year** saves you $60 a year and still has room for the browser image plus npm install spikes.

Either way you get the same KVM isolation, dedicated IPv4, and root access. If you already know your way around a terminal, the manual route is hard to argue with.

👉 [Open the RackNerd 8 GB KVM plan ($119.99/year) for one-click deployment](https://my.racknerd.com/aff.php?aff=11397&pid=956)

## What RackNerd costs over three years — compared to the usual suspects

This is where the renewal policy actually matters. Intro pricing dominates most comparison tables, but OpenClaw is something you typically run for years, so the second and third year deserve equal billing.

Take Hostinger's KVM 1 as an example: $6.49/month on a two-year term, then renewing at $11.99/month. RackNerd's 4 GB special — with more vCPU cores (3 vs 1) and comparable RAM — works out to about $5.00/month, every year, no step-up. Or look at Hetzner's CPX22 (2 vCPU / 4 GB / 80 GB) at €19.49/month before VAT: an excellent month-to-month option that you can resize or delete freely, but roughly four times RackNerd's annual 4 GB price over a year if you're keeping the box running anyway.

The trade-off is real and worth stating plainly: Hetzner, Vultr, DigitalOcean, and Akamai bill monthly with no commitment, which is better if you might tear the server down in a few months. RackNerd's pricing only wins if you're in for the long haul — which, for a personal AI agent, most people are.

One pricing trap to avoid: annual specials can be upgraded mid-term by submitting a ticket and paying the difference, but moving from an annual special to a regular monthly plan puts you back on regular pricing. The locked rate doesn't follow you across product lines.

## The stuff other guides skip

A few RackNerd specifics that don't show up in the standard "specs and price" tables but affect the buying decision:

- **There's no standard money-back guarantee.** RackNerd doesn't advertise a 30-day no-questions refund on VPS plans. If you want commitment-free testing, a monthly-billed provider is the better fit; if you're confident in the workload, the annual specials are where the value is.
- **Renewal is manual by default.** Invoices generate about 14 days before expiry and arrive by email; you pay to renew. Auto-recharge exists in the client area but is switched off by default. If an expired service matters to you, don't ignore those emails — expired services get suspended, though data is retained for a grace period of days to about a week according to one long-term customer's account.
- **Every plan includes a dedicated IPv4.** Hetzner's cloud plans charge extra for IPv4 (roughly $0.60/month on top of the listed price, with IPv6-only by default) — a detail that can trip up services that don't handle IPv6. RackNerd bakes one IPv4 into every plan.
- **Discount codes won't apply here.** The specials are already the floor price and generally can't be stacked with promo codes; codes mainly apply to RackNerd's regular-priced products. Don't go hunting for a coupon on these — there isn't one.

## How RackNerd stacks up for OpenClaw specifically

Verified pricing as of late 2026, information only:

| Provider | Plan | vCPU / RAM / Disk | Price | Billing notes |
| --- | --- | --- | --- | --- |
| RackNerd | 4 GB KVM | 3 / 4 GB / 60 GB | $59.99/yr | Annual; renews at same rate |
| OVHcloud | VPS-1 | 2 / 4 GB / 40 GB NVMe | $4.54/mo | Annual upfront |
| Contabo | Cloud VPS 4 | 4 / 8 GB / 100 GB | €4.40/mo | Intro price, first 24 months (list €5.50) |
| Hostinger | KVM 1 | 1 / 4 GB / 50 GB NVMe | $6.49/mo | 2-year term; renews $11.99 |
| Hetzner | CPX12 | 1 / 2 GB / 40 GB | €11.49/mo | EU locations; before VAT; month-to-month |
| DigitalOcean | Basic | 1 / 2 GB / 50 GB | $12/mo | Month-to-month |

Where each one makes sense: Contabo's 8 GB at €4.40/month is compelling if you're also running n8n or other memory-hungry self-hosted tools alongside OpenClaw — just remember that price only holds for the first 24 months. Hetzner and DigitalOcean are the right call when you want hourly flexibility and might delete the server next month. Contabo and Hostinger also offer one-click OpenClaw deployment, if that's your preferred path. None of them, at their regular rates, match RackNerd's renewal pricing for a server you plan to keep for years.

## FAQ

**Do I need a GPU VPS for OpenClaw?**
No. OpenClaw calls a remote model provider for inference; the server only runs the agent. Only people running local models need dedicated hardware, and that's dedicated-server territory, not a VPS.

**Will RackNerd's special price go up when I renew?**
No. The specials page states the pricing is lifetime recurring — you renew at the same rate for as long as you keep the service. This is RackNerd's long-standing policy, corroborated by customers who've renewed multiple times at identical rates.

**Can RackNerd's 2 GB plan run OpenClaw?**
For a basic Telegram or Discord bot using the pre-built Docker image, yes — community testing puts typical agent memory usage well under 2 GB. For browser automation, multiple integrations, or background jobs running all day, step up to 4 GB or more.

**Which is better, the one-click app or manual Docker?**
The one-click app in ctrl is faster to start and shows credentials in-panel, but it's officially sized for 8 GB RAM. Manual deployment with the pre-built image runs comfortably on 4 GB. Manual also gives you control over image variants (like `slim` or `-browser`).

**What if I outgrow my plan?**
Submit an upgrade ticket in the client area and pay the difference. One caveat: moving off an annual special onto a regular monthly plan means regular pricing from then on, so pick your tier with a year or two of headroom in mind.

## The bottom line, by situation

Keeping a personal OpenClaw agent running year-round and happy to do the two-minute Docker setup → the **4 GB special at $59.99/year** is the value pick, and the renewal price stays put. Want the least friction possible and the one-click installer → 8 GB at $119.99/year. Just a lightweight chat bot to start → 2 GB at $35.99/year, upgrade later if it grows. Need month-to-month flexibility or a money-back window to experiment → a monthly-billed provider like Hetzner or DigitalOcean suits that better, at a higher steady-state cost.

One last check before ordering: pick a datacenter location that's close to whoever (or whatever) talks to your agent most — latency matters more for an interactive bot than raw specs do.

👉 [Grab the RackNerd 2 GB KVM special at $35.99/year](https://my.racknerd.com/aff.php?aff=11397&pid=953)
