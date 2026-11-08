# Cloud Server Company Guide: SpinServers' VPS and Dedicated Lineups, What 30TB of Transfer Really Means, and Who Should Look Elsewhere (Full Spec Table)

SpinServers sells KVM virtual servers from 4 to 32 vCPU and single-tenant dedicated machines with 10Gbps ports, from data centers in Dallas and the San Jose / Silicon Valley area. Every VPS ships with RAID-10 SSD storage, 8TB to 20TB of monthly transfer on a 1Gbps port, and one IPv4 plus one IPv6 address. That mix of cores, RAM and transfer per dollar is the reason to shortlist them. A managed stack, one-click app hosting, or outbound email that works on day one are the reasons to keep looking.

This guide covers what the lineup actually contains, how to size a plan without overpaying, and the cost items and restrictions that decide whether the monthly number you see is the monthly number you pay.

> **Most readers should start on the VPS line, not a dedicated box.** The 4 vCPU / 4GB plan with 80GB RAID-10 SSD and 8TB of transfer covers a handful of small sites or an internal API with room to spare. The 32 vCPU / 32GB Advanced plan is for parallel batch work, not for a busier website. VPS rates are quoted on the order page, so 👉 [check SpinServers' current VPS and dedicated pricing](https://bit.ly/Spinservers) before you compare it against anyone else's.

## The KVM VPS lineup

Six configurations, all on a 1Gbps port, all including one IPv4 and one IPv6 address.

| Plan | vCPU | RAM | Storage | Monthly transfer | Order |
| --- | --- | --- | --- | --- | --- |
| Simple Application Server | 4 (Intel) | 4GB DDR4 | 80GB RAID-10 SSD | 8TB | [View the 4 vCPU plan](https://bit.ly/Spinservers) |
| Simple Application Server | 6 | 8GB DDR4 | 120GB RAID-10 SSD | 8TB | [View the 6 vCPU plan](https://bit.ly/Spinservers) |
| Simple Application Server | 8 | 8GB DDR4 | 160GB RAID-10 SSD | 10TB | [View the 8 vCPU plan](https://bit.ly/Spinservers) |
| Simple Application Server | 12 | 12GB DDR4 | 160GB RAID-10 SSD | 10TB | [View the 12 vCPU plan](https://bit.ly/Spinservers) |
| Simple Application Server | 16 (Intel Xeon) | 16GB DDR4 | 320GB RAID-10 SSD | 15TB | [View the 16 vCPU plan](https://bit.ly/Spinservers) |
| Advanced Application Server | 32 (Intel Xeon) | 32GB | 500GB SSD | 20TB | [View the 32 vCPU plan](https://bit.ly/Spinservers) |

Two things stand out in that table. The jump from 8 vCPU to 12 vCPU buys you more RAM (12GB) but no extra disk or transfer, so if you are storage-bound rather than CPU-bound, the 12 vCPU tier does nothing for you. And storage scales far more slowly than cores: 4x the vCPU between the smallest and largest VPS gets you 6.25x the disk and 2.5x the transfer, which tells you these are built for compute-heavy, storage-light workloads.

## The dedicated server side

This is where the 10Gbps ports live. The monthly rates below come from a third-party roundup of SpinServers configurations, and they are indicative rather than gospel. Confirm the current number on the order page before you commit to anything.

| Configuration | CPU / RAM | Storage | Port and transfer | Price |
| --- | --- | --- | --- | --- |
| Entry | Intel E3-1280 v5, 4 cores / 32GB | 1TB NVMe | 10Gbps, 30TB | $49/mo |
| Standard | 2× E5-2630L v3, 16 cores / 256GB | 2× 1.6TB SSD | 10Gbps, 30TB | $89/mo |
| High | 2× E5-2696 v3, 36 cores / 128GB | 480GB SSD | 10Gbps, 30TB | $139/mo |
| Flagship | AMD EPYC 7702P, 64 cores / 512GB (to 1TB) | 240GB SSD + 7.84TB NVMe | 10Gbps, 100TB | $449/mo |
| San Jose base | 2× E5-2630L v3, 16 cores / 64GB | 1.6TB SSD | 500Mbps unmetered | $79/mo |
| San Jose upgrade | 2× E5-2630L v3, 16 cores / 256GB | 2× 1.6TB SSD | 1Gbps unmetered | $99/mo |
| San Jose deluxe | 2× E5-2683 v4, 32 cores / 512GB | 2× 3.84TB SSD | 10Gbps, 30TB | $239/mo |

Read the entry row carefully. Thirty-two gigabytes of RAM and a 1TB NVMe drive for $49 a month is a lot of machine, but the E3-1280 v5 is a four-core chip from the mid-2010s. If your workload cares about per-core speed, a current-generation 4 vCPU VPS will feel quicker than this box on single-threaded work. You are buying RAM and disk here, not clock speed. The $89 and $139 tiers make the same trade at 256GB of RAM, which is the point where virtualization hosts and in-memory databases start to make sense.

## A 10Gbps port with a 30TB cap is a burst port

Run the arithmetic before you get excited about the port speed. Ten gigabits per second is about 1.25GB per second, and sustaining that for a month would move somewhere around 3.2 petabytes. A 30TB monthly allowance is roughly 1% of that ceiling. So the port exists for bursts: pushing a container image, seeding a backup, migrating a few hundred gigabytes in an afternoon. It is not a promise that you can saturate 10Gbps for 30 days.

The "unmetered" plans have the same kind of ceiling, just expressed differently. A 500Mbps unmetered port tops out near 160TB a month if you run it flat out around the clock; 1Gbps unmetered doubles that. Unmetered means no per-gigabyte invoice, not unlimited capacity.

## Port 25 is closed by default on every VPS

SpinServers states plainly that port 25, the SMTP sending port, is closed on all of its VPS plans to limit IP abuse. Opening it requires a support ticket with a justification, and the company reserves the right to refuse.

If your application sends password resets, order confirmations, or alert emails, plan for an external transactional email service and route through that from the start. If you intend to run a mail server, get written approval before you pay, because a refused ticket after deployment leaves you with a box that cannot do the job you bought it for.

👉 [Open the SpinServers order page and match a plan to your workload](https://bit.ly/Spinservers)

## What the sticker price leaves out

No provider's headline rate is the whole bill, and the gap is usually made of small line items. Here is what those look like elsewhere in the market, so you know what to ask about.

**Extra IP addresses.** Atlantic.Net includes one IPv4 per server and charges $3.65 per month for each additional one. Across a dozen machines that is $525 a year in addresses alone. Ask what extra IPv4s cost here before you plan an architecture that needs them.

**Snapshots are not backups.** UpCloud includes daily snapshots on several plan families with a 24-hour retention window and sells longer retention at 7, 30, or 365 days. Donweb deletes snapshots after roughly 28 days and says outright that they are not a long-term backup method. Whichever provider you pick, check the retention window and keep an independent copy of anything you cannot rebuild.

**Powered-off does not always mean stopped.** UpCloud's Cloud Native plans bill CPU and memory only while an instance is running, but storage and IP addresses keep billing because they stay allocated. If you plan to park a server between jobs to save money, confirm whether that actually saves anything.

**Custom configurations take time.** A third-party write-up notes that high-spec custom builds can take three to six business days to deliver. Listed configurations move faster. If you have a migration window, order before it opens.

Do the annual math on whatever you choose. The $49 dedicated entry is $588 a year; the $89 tier is $1,068; the $449 EPYC box is $5,388. Cloudzy's 4GB VPS at $14.48 a month is about $174 a year, but that is a 4GB machine with 5TB of transfer, not a 32GB single-tenant server. Comparing those two numbers directly tells you nothing.

## Which plan fits which job

**A few low-traffic sites, a cron-driven API, a staging environment.** The 4 vCPU / 4GB VPS. The 8TB of transfer is far more than this workload will touch, and more cores will sit idle.

**Container builds, CI runners, media transcoding, game servers.** The 8 or 12 vCPU VPS with 10TB. These workloads actually consume cores and can push real transfer, which is what the plan is priced for.

**Parallel batch jobs, rendering, a dozen services on one host.** The 32 vCPU / 32GB Advanced plan. Buy it when you can name the processes that will keep 32 cores busy; otherwise the extra cores cost money and do nothing.

**A big in-memory database, a virtualization host, anything RAM-bound.** The $89 or $139 dedicated configurations at 256GB. This is the only sensible reason to accept older Xeon cores.

**Sustained throughput to a large audience.** The $449 EPYC configuration with 100TB, or a colocation arrangement, since a single 30TB cap will not carry a video origin.

## When SpinServers is the wrong choice

Skip it if you want a managed database, S3-compatible object storage, or self-service private networking bundled with the server. The VPS pages cover compute, storage, transfer and IPs; nothing there describes a managed services layer, so if your stack depends on one, confirm in writing that it exists before ordering. Cloudzy's own customers have publicly asked for exactly that layer, which tells you how common the gap is among value-focused hosts.

Skip it if you need outbound email working immediately, for the port 25 reason above.

Skip it if you want many small instances billed by the hour with an API to spin them up and tear them down. That is a different product category, and the per-hour, API-first providers do it better.

Skip it if single-thread performance is the priority. The low-end dedicated hardware is a decade old, and a modern 2 vCPU cloud instance will beat a 2015-era 4-core Xeon on latency-sensitive work.

Skip it if you need a written money-back guarantee before you pay. No refund window is published on the pages I could check, so ask sales to confirm one in writing, or start with the smallest VPS rather than a $449 commitment.

## Two reference points from the rest of the market

**UpCloud** starts at €3 per month for 1GB RAM, 1 vCPU, 10GB storage and a 250Mbit/s port, with deployment quoted at 45 seconds and zero fees on outgoing transfers. One detail that gets lost in comparisons: the headline processor is a 5th-gen AMD EPYC 9575F, but the Starter tier is explicitly described as using previous-generation AMD CPUs and standard-performance SSD. The SLA is also quoted two ways on the same page, 99.99% for Starter plans and 99.999% at brand level, so read the number that applies to the tier you are buying. The vendor's own Geekbench figure claims over 37% better performance for a 2 vCPU / 4GB EPYC 9575F configuration than the previous generation.

**Cloudzy** advertises plans from $2.48 per month, displayed next to a $4.95 list price at 50% off, with no statement on the page about what happens at renewal. A 4GB / 2 vCPU / 120GB NVMe configuration with 5TB of transfer is $14.48 against a $28.95 list price. It includes a 14-day money-back guarantee, hourly billing, no egress fees, and 13 regions. The vendor publishes Geekbench 6 single-core results of 2080 for itself against 1424 for AWS, 1380 for Vultr and 1188 for DigitalOcean; those are vendor-run figures, not independent tests.

The split is fairly clean. A 32GB single-tenant box with a 10Gbps port and 30TB of transfer has no equivalent at either of those providers at that price. Cheap, small, hourly-billed instances with a refund window are their territory, not SpinServers'.

## FAQ

**Is a cloud server company the same thing as a VPS host?**
Not exactly, though the terms overlap. A VPS is one virtual slice of a physical machine. A cloud server usually implies the instance can move between hardware and be resized or billed by the hour. SpinServers' VPS line is KVM virtualization on RAID-10 SSD with fixed monthly transfer allowances; its dedicated line is a whole physical machine. Both are self-managed infrastructure, which is what most people searching for a cloud server company actually need.

**Monthly or hourly billing?**
SpinServers' published pricing is monthly. If you need per-hour billing for short-lived workloads, that is a feature to confirm with sales rather than assume.

**Can I get a refund or a trial?**
No refund window is published on the pages I could verify. Ask before paying, and if you want to limit exposure, start on the smallest VPS instead of a dedicated contract.

**How long until the server is online?**
Listed configurations are positioned as fast provisioning. Custom and high-spec builds are reported to take three to six business days, so order ahead of a migration deadline.

## Picking between them

If you need one large machine with a lot of RAM, a 10Gbps burst port and a generous transfer allowance, and you are comfortable running the OS and application stack yourself, start with the 4 vCPU VPS to validate the network, then move up to a dedicated configuration once you know your real memory footprint. If you need small hourly instances, a published refund window and no egress fees, Cloudzy fits that description better. If you want euro-denominated pricing, sub-minute deployment and zero transfer fees on a light workload, UpCloud's Starter tier is the obvious comparison.

Two things to confirm before you pay anywhere: whether extra IPv4 addresses and outbound transfer overages are billed separately, and whether the provider will open port 25 for your use case.

👉 [Start from SpinServers' order page and pick the smallest plan that clears your workload](https://bit.ly/Spinservers)
