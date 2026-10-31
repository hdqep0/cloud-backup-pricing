# cloud based data backup: real prices, plan differences, and how to pick one without overpaying

Search "cloud based data backup" and you mostly get two things: vendor pages that never show a price, and "best of" lists that recycle the same ten logos. The actual decision is simpler than that. You need to know (a) what kind of backup you're doing, (b) what the storage and machines really cost per year, and (c) which limits are buried in the checkout page. This article walks through all three using Sharktech's currently published pricing — a hosting provider that sells both raw S3-compatible object storage (from $4.90/TB per month) and a hosted Acronis Cyber Protect backup service (from $4/month).

> **Quick verdict:** If you just need somewhere to push backups from your own software (rclone, Nextcloud, S3 Browser), Sharktech's S3 Object Storage at **$4.90/TB/month with 1TB of monthly transfer included** is the straightforward pick. If you want a managed, agent-based solution that protects whole machines with schedules and retention policies, the Acronis backup service starts at **$4/month for one machine with 200GB**. 👉 [Check Sharktech's current backup pricing](https://bit.ly/SharKtech)

## Cloud backup and cloud storage: a 60-second orientation

Most confusion around cloud based data backup comes from mixing up two products that solve different problems.

Cloud storage (Dropbox, Nextcloud, and similar) is built for everyday access and collaboration. Cloud backup is built for recovery after something goes wrong — hardware failure, ransomware, accidental deletion. A proper backup service gives you granular file restore, full machine recovery, and point-in-time rollback; a sync folder usually only offers short version history. This distinction is well covered elsewhere, so I'll keep it short: if your question is "can I get my server back to Tuesday morning," you want backup. If it's "where do I keep my photo library," you want storage.

There's a third layer worth knowing about: object storage. It's raw capacity accessed through an API (S3-compatible, in Sharktech's case), and it's where backup software actually puts your data. Many backup tools — including open-source ones like rclone — can write directly to it.

## Sharktech's backup-related plans, in full

Sharktech doesn't sell one monolithic "backup plan." It sells two distinct products that cover most cloud based data backup scenarios, plus cloud servers you can run backup infrastructure on. Here's everything currently published.

### S3 Object Storage — raw capacity for backups and archives

| Plan | Storage | Monthly price | Transfer included | Get pricing |
| --- | --- | --- | --- | --- |
| S3 Storage 1TB | 1 TB | $4.90/mo | 1 TB/month free | [Order 1TB S3 storage](https://bit.ly/SharKtech) |
| S3 Storage 10TB | 10 TB | $49/mo | 1 TB/month free | [Order 10TB S3 storage](https://bit.ly/SharKtech) |
| S3 Storage 100TB | 100 TB | $490/mo | 1 TB/month free | [Order 100TB S3 storage](https://bit.ly/SharKtech) |

Beyond the included 1TB of monthly outbound transfer, egress costs $0.90/TB. The architecture is high-capacity HDD with an NVMe cache layer — Sharktech explicitly positions it as capacity-first storage, and explicitly says it's a poor fit if you need boot disks or low-latency block storage. You connect with tools like Rclone, Cyberduck, Nextcloud, or S3 Browser, or through the API directly. It supports object versioning, object locking (useful for ransomware-resistant retention), lifecycle rules, and multipart uploads.

If you need more than 100TB of storage or 300TB of monthly transfer, that goes through their sales team as a custom quote.

### Acronis Cyber Protect backup — managed, machine-based protection

This is a hosted Acronis Cyber Protect service: an agent-based backup platform that protects physical servers, VMs, and cloud workloads across Windows, Linux, and macOS, managed through a central web console. Pricing is built from two components — number of protected machines and backup capacity (starting at 200GB) — with four billing cycles available.

| Protected machines | Monthly | Annual | Effective monthly on annual | Get pricing |
| --- | --- | --- | --- | --- |
| 1 machine | $4/mo | $24/yr | ~$2/mo | [Start with 1 machine](https://bit.ly/SharKtech) |
| 5 machines | $16/mo | $96/yr | ~$8/mo | [Protect 5 machines](https://bit.ly/SharKtech) |
| 10 machines | $30/mo | $180/yr | ~$15/mo | [Protect 10 machines](https://bit.ly/SharKtech) |
| 25 machines | $70/mo | $420/yr | ~$35/mo | [Protect 25 machines](https://bit.ly/SharKtech) |
| 50 machines | $130/mo | $780/yr | ~$65/mo | [Protect 50 machines](https://bit.ly/SharKtech) |

The final recurring price depends on the capacity and protection options you select at checkout — the cart lets you pick machine count, capacity, and add-ons like File Sync & Share, which is priced separately. Sharktech claims the service costs about half what competitors charge for equivalent Acronis hosting; that's their own comparison, so treat it as a claim, but the baseline numbers above are directly from their order page.

One machine backed up for $4/month (or $24/year) with 200GB included is an unusually low entry point for Acronis-based protection. Most Acronis hosting resellers start several times higher.

## What you actually pay per year

This is where most backup articles stop at the monthly headline number, and where the real differences show up.

**Acronis service, monthly vs annual.** One protected machine costs $4 monthly — $48 over a year. The same machine on the annual cycle is $24. That's a 50% cut, and it holds across the range: 10 machines run $360/year on monthly billing versus $180/year on annual. If you're committing to backup anyway (and you should be), monthly billing on this service is mostly paying double for flexibility you won't use.

The overage pricing follows the same logic. Extra capacity beyond the included 200GB is billed per GB at rates that drop with longer cycles — $0.02/GB monthly, down to $0.12/GB on the annual tier per the announcement pricing (which maps to $0.02/GB/month equivalent). In plain terms: if your backup set is going to grow, pick the longest billing cycle you're comfortable with before you buy, not after.

**S3 storage, with egress factored in.** $4.90/TB/month sounds cheap until you ask what restores cost. Sharktech includes the first 1TB of monthly transfer free, then charges $0.90/TB. If you back up 1TB monthly and rarely restore, your effective cost stays near the headline price. If you do a full 10TB restore after an incident, that's roughly $8 extra in egress for that month — noticeable, but not brutal. The key point for comparison shopping: always price egress alongside storage, because at scale it's often the bigger line item.

**The bill nobody reads.** On Sharktech's Public Cloud (if you go the DIY route and run backup servers yourself), the Small, Medium, and Large plans have resource ceilings — but the FAQ specifically states these are not spending caps. Usage above your allocation is billed at published rates. If budget predictability matters more than elasticity, their Dedicated Cloud mode is the fixed-price alternative.

## S3 storage or Acronis backup: which one do you actually need?

The two products overlap less than you'd think. Here's the decision rule.

**Choose S3 Object Storage if:**

- You already run backup software and just need a destination. Rclone, S3 Browser, and Nextcloud are officially named as compatible on Sharktech's page.
- You're backing up large, mostly static datasets — archives, media libraries, logs, database dumps.
- Cost per terabyte is your main constraint and you're comfortable managing retention yourself.
- You want ransomware-resistant retention: object locking prevents deletion of backup versions even with credentials compromised.

**Choose Acronis backup if:**

- You want whole-machine protection — full servers, VMs, or workstations — with a single agent, not file-by-file syncing.
- You need scheduling and retention policies managed centrally, with reporting that shows failed backup jobs.
- You want restore granularity from a single file up to a complete machine, without scripting it yourself.
- You're protecting multiple machines: the per-machine pricing scales predictably, and 25 machines at $420/year works out to about $1.40 per machine per month.

**Choose neither and DIY it if** you already run your own backup infrastructure. Sharktech's OpenStack-based Public Cloud (from $39/month base in the Small tier, deployable in Los Angeles, Las Vegas, Denver, Chicago, or Amsterdam) or their bare-metal servers let you run whatever backup stack you prefer — but at that point you're buying infrastructure, not a backup service.

One honest limitation worth stating: the S3 storage is HDD-plus-cache, so it's built for throughput on large objects rather than latency. Random small-file workloads and anything needing boot-disk behavior will frustrate you. Sharktech says this themselves on the product page, which is refreshing, but it's still something to respect when planning.

## Details to check before you order

A few things live in checkout flows and FAQ pages rather than the marketing copy. Worth 60 seconds each:

1. **The Acronis cart is à la carte.** Machine count, capacity, and add-ons like File Sync & Share are separate line items. Your price only becomes final when you've confirmed the full configuration in the customer portal — don't assume the $4 headline covers your exact setup.
2. **Application and database protection depends on your license tier.** The Acronis page notes that protection for specific applications and databases, plus advanced security and disaster-recovery features, varies with the license you select. If you're backing up SQL Server or Exchange, verify workload support before paying.
3. **No published free trial or refund terms for the backup services.** Sharktech's third-party reviews have flagged the absence of a money-back guarantee on their hosting products in general. The backup pages don't publish trial or refund terms either way, so size your first purchase conservatively.
4. **Data location matters for compliance.** Sharktech operates data centers in Denver, Chicago, Los Angeles, Las Vegas, and Amsterdam. If you have data-residency requirements, pick your deployment region deliberately rather than accepting the default.

On the reliability side: the provider runs all services with 60Gbps DDoS protection included, and an independent hosting review that benchmarked their network recorded a 5.33Gbps download with no packet loss and no downtime during the test period. One long-term customer who collocates hardware in their Denver H5 facility specifically uses it for "bandwidth-intensive stuff, such as backups" and describes the connectivity as flawless — that's from the testimonials published on Sharktech's own site, so weigh it accordingly.

## FAQ

**Is $4.90/TB for object storage actually cheap?** Yes, by the standards of the big public clouds, where standard-tier object storage commonly runs several times that per TB before egress fees. Sharktech's version trades away latency (HDD-based with NVMe cache) to get there, which is exactly the right trade for backup and archive workloads.

**Can I use my existing backup software with Sharktech's S3 storage?** If it speaks S3, very likely yes. Sharktech specifically names Rclone, Cyberduck, Nextcloud, and S3 Browser as compatible, and the API is standard S3-compatible. Test with a small dataset first — that costs pennies and settles the question definitively.

**Does the Acronis service back up my phone or laptop too?** The service covers physical servers, VMs, and cloud workloads across Windows, Linux, and macOS through the Acronis agent. Mobile device backup and File Sync & Share fall under optional services priced separately — check the itemized cart before ordering.

**What happens if my backup set outgrows 200GB?** You adjust capacity in the customer portal, and extra storage bills per GB at your billing cycle's rate. Since the annual cycle's per-GB rate is the lowest, growth is cheapest to absorb on longer cycles — another argument for starting annual if the service fits your needs.

## Where to land

Still stuck? Match your situation to one of these:

- **One or two servers, want it handled** → Acronis at 1–5 machines, annual billing. $24/year for one machine is the whole decision. 👉 [Set up Acronis backup from $24/year](https://bit.ly/SharKtech)
- **Existing backup tooling, need cheap destination storage** → S3 Object Storage, 1TB plan to start, scale up when your dataset says so. 👉 [Compare Sharktech's storage and backup plans](https://bit.ly/SharKtech)
- **Anything over 100TB, or unusual requirements** → skip the standard plans and get a custom quote from their sales team instead of forcing your workload into a tier that doesn't fit.

Before clicking buy, do two things: total up your real backup set size (including retention copies, which typically triple your storage need), and decide your billing cycle based on how long you'll realistically keep the service. Both numbers are already on the checkout page — you just want to know them *before* the first invoice, not after.
