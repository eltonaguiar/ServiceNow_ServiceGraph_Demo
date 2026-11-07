# ServiceNow_ServiceGraph_Demo

> [!WARNING]
> This is an attempt to be no cost, but it may instead be low cost. See AWS/Azure, follow the balance warnings, etc.

Following this Lab I generated with AI, specifically to teach the Service Graph Connector, and get something working for individuals who don't have SCCM / Windows Server. So if you have a typical Windows 11 Machine or maybe Linux...

**Lab:** https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/00-prerequisites/

Below is a condensed version from my actual experience in running this lab.

---

## PDI

Get your PDI at [developer.servicenow.com](https://developer.servicenow.com).

---

## Setup your AWS Account

> [!WARNING]
> Credit Card Required. Be careful, and stop the instance on the same day.

### Free AWS account (Amazon Web Services)

1. Create the account at [aws.amazon.com/free](https://aws.amazon.com/free). A card is required for identity verification.
2. **Immediately** set a budget alarm: **Billing and Cost Management → Budgets → Create budget** → Zero-spend budget or a fixed **$1–5** monthly budget with an email alert at 50%.
3. Choose one region and stick to it for the whole lab (for example `ca-central-1`). Multi-region imports multiply the data and the surprises.
4. Note your **12-digit account ID** (top-right account menu).

### What “free tier” actually means in 2026

Accounts created after mid-2025 are on a **credit-based Free plan**, not the old twelve-month allowance.

> [!IMPORTANT]
> **You start with a credit balance.** The plan ends when the credits run out or after about six months, whichever comes first. After that, resources are stopped or billed depending on the choice you made at signup.
>
> A public **IPv4 address is billed per hour** on most account types. Networking is where the surprise charges come from, not the instance.
>
> `t3.micro` / `t2.micro` is only “free” inside a monthly hour allowance and only in a new account. An instance left running past the allowance bills normally.

<details>
<summary><strong>How much does it actually cost? (AWS vs Azure)</strong></summary>

### Approximate rates (late 2026, cheap US region)

| Item | AWS (t3.micro) | Azure (B1s) |
|------|----------------|-------------|
| VM compute | ~$0.0104/hour | ~$0.0104/hour |
| Public IPv4 | $0.005/hour (even if stopped) | ~$0.004–0.005/hour |
| Small disk | a few cents | a few cents |
| New-account free tier | $100–$200 credits (6 months) | $200 credit (30 days) + 750 free B1s hours/month for 12 months |

### Example costs for one lab VM

| Duration | Running | Stopped / deallocated (IP + disk kept) |
|----------|---------|----------------------------------------|
| **8 hours** | ~$0.12 | ~$0.04 |
| **2 days** | ~$0.75–$1.00 | ~$0.25–$0.40 |
| **1 month** | ~$11–12 | ~$4 |

**Notes**
- On AWS, a public IPv4 keeps charging after you stop the instance.
- On Azure, you must **deallocate** the VM (not just shut down inside the OS) to stop compute charges. Static IPs and managed disks still cost money.
- These amounts are usually covered by free-tier credits on a new account. The risk is forgetting resources for weeks.

</details>

**Rule for this lab:**

> Create → verify → stop the instance → delete the stack the same day.  
> Treat “I will clean up later” as the failure mode. Lab 11 is designed to take four minutes, not to be a chore you postpone.

The budget alarm is your only early warning, which is why it is step 2 and not an afterthought.

---

## Optional — Setup your Free Azure Account

*(Lab 8 only)*

1. Create the account at [azure.microsoft.com/free](https://azure.microsoft.com/free).
2. Set a **Cost alert** in Cost Management → Budgets ($1–5).
3. Do **not** upgrade to pay-as-you-go for this lab.

You can complete Labs 0–7 and 9–11 without Azure.
