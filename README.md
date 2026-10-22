# secure vps hosting: What the Provider Actually Covers, What Costs Extra, and the First-Hour Hardening That Keeps Bots Out

Search for secure vps hosting and you get a wall of pages promising DDoS mitigation, encryption, and enterprise-grade everything. The useful question is narrower: which protections come with the price, which are a paid add-on, and what is still your job the moment the box boots.

Security on a VPS comes from two separate places. The provider handles the network edge and the recovery console. You handle SSH, the firewall, patching, and the application stack. A host can sell you one half. Nobody does the other half for you on an unmanaged box.

GoMami is a reasonable case study here because its documentation states where that line sits, including the parts that don't flatter the sales page.

> **Quick pick:** for a China-optimised box without overthinking it, the HKG Pulse Mini (2 vCPU / 4GB / 60GB NVMe / 1TB traffic on a 1Gbps port, around $59/mo) is the sensible default for a website or API. The $29 JPN or SIN Pulse Nano is the cheapest legitimate entry point if you don't specifically need Hong Kong. 👉 [Compare GoMami's current plans and prices](https://bit.ly/Gomami)

## What does "secure VPS hosting" actually include?

It includes two layers, and they fail independently. The provider layer is DDoS scrubbing, network routing, the hypervisor, and out-of-band access when you lock yourself out. The server layer is everything from your SSH config to your database bind address. Most breaches you read about happen in the second layer, on machines whose provider layer was perfectly fine.

A fresh instance with a public IPv4 address gets found fast. Automated scanners sweep address ranges continuously, and the auth log starts collecting failed root logins long before anyone has a reason to target you personally. What matters is whether the defaults you leave in place survive that background noise.

So the honest way to evaluate a host is to ask three questions:

1. What network protection is included by default, and what happens when an attack exceeds it?
2. What can you still do when SSH is broken? Is there an out-of-band console, or do you reinstall?
3. What's the refund or exit path if either answer turns out badly?

GoMami's documentation addresses all three, and the answers are specific enough to act on.

## What GoMami's protection covers, and what it doesn't

The free tier is basic scrubbing. GoMami's homepage advertises "up to 600 Gbps mitigation capacity," while the network docs describe every instance as getting best-effort basic scrubbing at no cost. The **600 Gbps professional scrubbing is a paid add-on** you arrange with sales. Basic protection auto-detects anomalous traffic and needs no configuration, but its capacity "varies depending on attack scale and network conditions."

The part that actually affects decisions is the blackhole policy:

> When an attack on your instance starts affecting other customers on the same network segment, the system null-routes your IP for **30 to 60 minutes**. It releases automatically. Manual intervention is not possible. During that window, all inbound and outbound traffic to the IP is dropped.

That's a normal industry trade-off rather than a scandal: the provider protects the shared segment. But two consequences deserve thinking about before you buy.

A large attack means your service goes dark for up to an hour, regardless of how well you've hardened the box. And an IP blackholed by DDoS is **explicitly excluded from refunds** — which is the opposite of what most people assume "secure hosting" means.

Mitigation covers the usual suspects across layers 3, 4 and 7: UDP, SYN, ACK and ICMP floods, DNS and NTP amplification, HTTP floods, CC attacks and Slowloris. Layer 7 policies vary by node and by protection tier, so don't assume identical behaviour across Hong Kong, Japan and Singapore.

If volumetric downtime would end your business, price the paid scrubbing tier before committing, or put a CDN in front and treat the origin IP as disposable.

## The full plan lineup

Prices below are the currently reported monthly rates. GoMami's store pages list configurations and let you choose a billing cycle at checkout, but the listing pages themselves don't print prices — so treat every figure as something to confirm on the order screen.

### Hong Kong — Turin

| Plan | vCPU / RAM / NVMe | Traffic / Port | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- |
| Turin.Mini | 2 / 4GB / 100GB | 1TB / 2Gbps | $69 | [ Order](https://bit.ly/Gomami) |
| Turin.Air | 4 / 8GB / 140GB | 2TB / 2Gbps | $129 | [ Order](https://bit.ly/Gomami) |
| Turin.Pro | 6 / 16GB / 180GB | 5TB / 5Gbps | $299 | [ Order](https://bit.ly/Gomami) |
| Turin.Ultra | 12 / 32GB / 220GB | 10TB / 5Gbps | $599 | [ Order](https://bit.ly/Gomami) |

*Prices are third-party-compiled current public quotes; the checkout page is authoritative.*

Turin runs on newer, higher-clock hardware and is the only line available in Hong Kong with that platform. Pro and above support Windows.

### Hong Kong — Pulse

| Plan | vCPU / RAM / NVMe | Traffic / Port | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- |
| Pulse.Nano | 2 / 2GB / 40GB | 500GB / 1Gbps | $49 | [ Order](https://bit.ly/Gomami) |
| Pulse.Mini | 2 / 4GB / 60GB | 1TB / 1Gbps | $59 | [ Order](https://bit.ly/Gomami) |
| Pulse.Air | 4 / 8GB / 80GB | 2TB / 1Gbps | $119 | [ Order](https://bit.ly/Gomami) |
| Pulse.Pro | 8 / 16GB / 100GB | 5TB / 3Gbps | $269 | [ Order](https://bit.ly/Gomami) |
| Pulse.Ultra | 12 / 32GB / 300GB | 10TB / 3Gbps | $499 | [ Order](https://bit.ly/Gomami) |

*Prices are third-party-compiled current public quotes; the checkout page is authoritative.*

Pulse uses the same China-optimised routing as Turin on slower silicon. For most website workloads the routing is what you're paying for, and this is where the price curve is flattest.

### Japan and Singapore — Pulse

| Plan | vCPU / RAM / NVMe | Traffic / Port | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- |
| Pulse.Nano | 2 / 2GB / 40GB | 500GB / 1Gbps | $29 | [ Order](https://bit.ly/Gomami) |
| Pulse.Mini | 2 / 4GB / 60GB | 1TB / 1–1.5Gbps | $49 | [ Order](https://bit.ly/Gomami) |
| Pulse.Air | 4 / 8GB / 80GB | 2TB / 1Gbps | $89 | [ Order](https://bit.ly/Gomami) |
| Pulse.Pro | 8 / 16GB / 100GB | 5TB / 3Gbps | $169 | [ Order](https://bit.ly/Gomami) |
| Pulse.Ultra | 12 / 32GB / 300GB | 10TB / 3–5Gbps | $338 | [ Order](https://bit.ly/Gomami) |

*Prices are third-party-compiled current public quotes; the checkout page is authoritative.*

Japan and Singapore currently list identical prices. The port speed on Mini and Ultra differs between the two nodes, so check the store page for the specific plan rather than assuming they match.

### Los Angeles — Pulse

| Plan | vCPU / RAM / NVMe | Traffic / Port | Price (USD/mo) | Order |
| --- | --- | --- | --- | --- |
| LAX.Pulse.Nano | 2 / 2GB / 40GB | 1TB / 1Gbps | $29 | [ Order](https://bit.ly/Gomami) |
| LAX.Pulse.Mini | 2 / 4GB / 60GB | 2TB / 1Gbps | $59 | [ Order](https://bit.ly/Gomami) |
| LAX.Pulse.Air | 4 / 8GB / 80GB | 4TB / 2Gbps | $129 | [ Order](https://bit.ly/Gomami) |
| LAX.Pulse.Pro | 6 / 16GB / 100GB | 8TB / 3Gbps | $259 | [ Order](https://bit.ly/Gomami) |
| LAX.Pulse.Ultra | 12 / 32GB / 300GB | 15TB / 5Gbps | $599 | [ Order](https://bit.ly/Gomami) |
| LAX.Pulse.Titan | 12 / 32GB / 600GB | 30TB / 10Gbps | $999 | [ Order](https://bit.ly/Gomami) |

*Prices are third-party-compiled current public quotes; the checkout page is authoritative.*

Los Angeles gives noticeably more traffic per dollar than the Asian nodes, and it's the only region with a 10Gbps tier.

### Peak X5 and Forge

Public pricing for these two lines wasn't verifiable at the time of writing, so there's no price column here. Both are listed on GoMami's store with full configurations; confirm the number on the config page or with support before ordering.

| Plan | Type | Hardware | Traffic / Port | Order |
| --- | --- | --- | --- | --- |
| Peak.X5.Mini | VPS | 2 vCPU / 4GB / 40GB NVMe | 1TB / 2Gbps | [ Order](https://bit.ly/Gomami) |
| Peak.X5.Air | VPS | 4 vCPU / 8GB / 60GB NVMe | 2TB / 2Gbps | [ Order](https://bit.ly/Gomami) |
| Peak.X5.Pro | VPS | 6 vCPU / 16GB / 80GB NVMe | 5TB / 5Gbps | [ Order](https://bit.ly/Gomami) |
| Forge.Mini | Dedicated | EPYC 7663, 56C/112T, 128GB / 960GB NVMe | 10TB included / 2Gbps | [ Order](https://bit.ly/Gomami) |
| Forge.Air | Dedicated | EPYC 7663, 56C/112T, 256GB / 4TB NVMe | 20TB included / 2Gbps | [ Order](https://bit.ly/Gomami) |

Forge is a dedicated server rather than a VPS, so the noisy-neighbour question disappears entirely. Traffic overage there is billed at $0.06/GB, and extra IPs cost $10 each, up to four per server.

## The controls that get you back in when something breaks

The most useful operational feature here is the out-of-band access. Every instance ships with a VirtFusion panel offering a browser VNC console, rescue mode, root password reset, and one-click OS rebuild.

That matters because one of the most common ways people lock themselves out of a VPS is during hardening itself. Disable password authentication before confirming key login works and you're stuck. Change the SSH port and forget to open it in the firewall and you're stuck. With a console you fix either in a couple of minutes. Without one, you reinstall and lose whatever was on the disk.

A few more operational details worth knowing before you buy:

- Deployment is automatic and typically finishes in 1–5 minutes after payment, with IP and credentials emailed to you.
- OS templates cover Ubuntu 22.04/24.04, Debian 11/12, CentOS Stream 9, AlmaLinux 8/9, and Windows Server 2019/2022. Anything else goes through a custom ISO.
- Plan upgrades and downgrades currently require a support ticket; there's no self-service resize.
- Reverse DNS is managed from the panel's Network tab.
- Payment works by card through Stripe, Stripe Alipay (CNY supported), or crypto including USDT. Account credit can be pre-funded and applied at checkout.

The panel's Storage tab lists backups, but retention, pricing, and self-service restore aren't documented publicly. If your workload can't tolerate an untested backup, schedule your own offsite copies instead of assuming.

## Hardening the box: the 30 minutes that do the most work

GoMami publishes its own hardening tutorial, and the parameters are worth copying as written because they're sane defaults rather than marketing copy:

1. SSH keys first, passwords second. Set `PubkeyAuthentication yes`, `PasswordAuthentication no`, `PermitRootLogin prohibit-password`. Confirm key login works in a second terminal before closing the first.
2. Move SSH off port 22. The docs use 2222. Allow the new port in the firewall before restarting SSH.
3. Install fail2ban with `maxretry = 5`, `bantime = 3600`, `findtime = 600`. Enough to knock out dumb brute-force loops without banning yourself.
4. Set UFW to deny inbound by default and allow only 2222/tcp, 80/tcp and 443/tcp.
5. Create a non-root sudo user (the docs use `deploy`) and work from that account.
6. Turn on the operating system's built-in automatic security updates rather than patching by hand.

Three failure modes show up over and over, and none of them are exotic:

- Adding an SSH key while leaving password auth enabled. The key does nothing and bots keep guessing.
- Opening a port to debug something and never closing it again. This is the quietest way a hardened server stops being hardened.
- Building a firewall rule set in a provider panel and never attaching it to the instance. It looks configured and enforces nothing.

One more that gets skipped: a database still listening on `0.0.0.0`. Your firewall is then one mistaken rule away from exposing it. Bind MySQL or Postgres to `127.0.0.1` unless something external genuinely needs access.

Account security belongs in the same conversation. If someone gets into the billing account behind your server, they can reset the root password or rebuild the machine. Whatever host you use, enabling two-factor on that account is worth more than most individual server-side tweaks. 👉 [Deploy a GoMami instance and harden it in the first hour](https://bit.ly/Gomami)

## What the docs don't tell you

There's no ISO 27001, SOC 2 or PCI DSS certification mentioned anywhere in GoMami's documentation or store, and no published uptime SLA figure. If you're buying against a compliance review, that gap is disqualifying. OVHcloud, for contrast, publicly lists PCI DSS, HDS and SecNumCloud certifications plus a dedicated incident response team — that's a different market segment, and GoMami isn't competing for it.

The homepage FAQ includes a question, "Is my data secure with your platform?", that as of writing has no published answer. Not damning, but it means you should evaluate the security posture from the docs rather than the sales copy.

Two operational limits live only in the FAQ:

- CPU throttling for noisy neighbours. Consistently maxing out CPU in a way that affects other users on the node can get you throttled. No threshold is published, so budget for it if you're running sustained compute.
- Traffic overage means throttling rather than automatic billing. Burn through your monthly quota and you get speed limits, not a surprise invoice. You can buy traffic packs instead: 100GB for $6, 1TB for $60, up to 20TB for $1,200.

## The refund window, and the traffic condition nobody mentions

GoMami offers a 24-hour refund window rather than a free trial. Within 24 hours of activation you can request a full refund on a newly purchased instance, once per user. Renewal orders don't qualify. Per the refund policy page, Alipay refunds land in 1–3 business days, cards in 5–10, and crypto in 1–3.

Three things narrow that window:

- IPs **blackholed by DDoS are not refundable**.
- The Terms of Service page adds a usage condition: consuming more than 10GB of traffic within the 24-hour period disqualifies you from the guarantee. Benchmarking a Nano plan can brush against that ceiling if you're not watching.
- The two official pages disagree on crypto. The refund policy page lists a 1–3 business-day crypto refund; the Terms of Service says crypto orders are final and non-refundable. If refundability matters to you and you're paying in USDT, ask support before you pay rather than after.

Once a refund is approved, the instance and its data are deleted immediately and permanently. Back up anything you want to keep before filing the ticket.

In practice, 24 hours is enough to benchmark network quality and confirm the panel behaves the way you expect. It isn't enough to judge a month of stability. 👉 [Start with a plan you can test inside the refund window](https://bit.ly/Gomami)

## The cost question that doesn't show up in most comparisons

GoMami's pricing isn't uniform across regions, and the gap is larger than most people expect. The same Pulse Nano — 2 vCPU, 2GB, 40GB, 500GB — costs $49 in Hong Kong and $29 in Japan or Singapore. That's a 69% premium for the Hong Kong route.

Hong Kong pricing was adjusted in June 2026, and the increases landed almost entirely on the mid and high tiers: Turin Pro moved from $199 to $299, Pulse Pro from $169 to $269, while Turin Mini stayed at $69 and Pulse Nano was introduced at $49 as a new low-end entry. Existing orders kept their original pricing at renewal.

On annual billing, GoMami's own billing FAQ says a yearly cycle typically saves 15–20% versus monthly, with exact discounts varying by product line. A third-party-reported coupon, `GOMAMI365`, is described as 20% off annual billing with the discount recurring at renewal. It isn't documented on GoMami's site, so type it into the promo code field at the cart and read the total before assuming it applied.

The practical takeaway: if your users are mostly in mainland China and latency is the product you're selling, Hong Kong is worth the premium. If you're running a backend, a build server, or anything where an extra 30–50ms of round trip doesn't change the outcome, Japan or Singapore at $29 does the same job for less.

## Who should not buy this

GoMami is a poor fit if you need published compliance certifications, a contractual uptime SLA, or managed security where somebody else patches the box. It's also a poor fit if you expect the 600 Gbps headline to apply to a $29 instance — that's the paid tier.

It's a reasonable fit if you want a China-optimised box with sub-50ms claimed RTT to the mainland, you're comfortable hardening Linux yourself, and you value having a VNC console and rescue mode for the day a config change goes sideways.

## Picking the right plan for your situation

Match the plan to what breaks first in your workload, not to the largest number in the table.

- Personal site, blog, or a lightweight API → Pulse Nano, $29 in Japan or Singapore, $49 in Hong Kong. 2GB gets tight once Nginx, PHP and a database are running, so keep the stack lean.
- A real website with a database and caching → Pulse Mini at $49–$59. Double the RAM and double the traffic for $10–$20 more is the best value in the lineup.
- Single-core-sensitive workloads — game servers, real-time apps, anything latency-bound → Turin Mini at $69. Turin runs on newer, higher-clock hardware; Pulse trades clock speed for price on identical routing.
- Sustained CPU load or heavy concurrency → Turin Air or above, or Forge if you want dedicated silicon and no shared-node throttling.
- High-bandwidth serving → LAX Pulse, where $29 buys 1TB and the Titan tier reaches 30TB on a 10Gbps port.

Two things to check on the checkout screen before you pay: the price for the billing cycle you've chosen, and whether the promo code you're trying actually applies. GoMami adjusts prices by region, and the figures here will drift.

If you're still deciding, start with the cheapest plan in the region closest to your users, run your own benchmarks inside the 24-hour window, and size up only after you've seen how the box behaves under your real workload. 👉 [Check current GoMami pricing and pick a node](https://bit.ly/Gomami)
