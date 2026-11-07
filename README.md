# ServiceNow_ServiceGraph_Demo

Following this Lab I generated with AI, specifically to teach the Service Graph Connector , and get something working for individuals who don't have SCCM / Windows Server. So if you have a typical Windows 11 Machine or maybe Linux , ..


https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/00-prerequisites/

# PDI
Assuming you have a PDI, if not
1. Personal Developer Instance (PDI)
Sign in at developer.servicenow.com → Request Instance.
Wait for the Ready state, then note the instance URL (https://devNNNNNN.service-now.com) and the admin password. Copy both into a scratch note.
Log in and confirm you hold admin.
PDIs hibernate after a period of inactivity and the URL can change when they wake. If you get an XML “instance not found” page, wake the instance first — it is not a connector problem.


If the PDI hibernates in the middle of the lab
This will happen, usually right after Lab 3. It is not a connector fault, and you do not need to start over:

Go to developer.servicenow.com → Manage instance → Wake. Wait for Ready.
Compare the URL with the one in your notes. It often changes. If it did, every SN_INSTANCE value in your shells and scripts is now wrong.
Re-login with the same admin password.
Re-run the connection Test from Lab 3 before trusting any import. Credentials records and connection aliases can be invalidated by the wake.
Re-run one schedule manually (Lab 4) and compare counts with the verifier — a second import should update existing CIs, not duplicate them.

# Setup your AWS Account

Free AWS account (Amazon Web Services)
Create the account at aws.amazon.com/free. A card is required for identity verification.
Immediately set a budget alarm: Billing and Cost Management → Budgets → Create budget → Zero-spend budget or a fixed $1–5 monthly budget with an email alert at 50 %.
Choose one region and stick to it for the whole lab (for example ca-central-1). Multi-region imports multiply the data and the surprises.
Note your 12-digit account ID (top-right account menu).
What “free tier” actually means in 2026
Accounts created after mid-2025 are on a credit-based Free plan, not the old twelve-month allowance. The practical consequences:

You start with a credit balance. The plan ends when the credits run out or after about six months, whichever comes first. After that, resources are stopped/billed depending on your choice at signup.
A public IPv4 address is billed per hour on most account types. Networking is where the surprise charges come from, not the instance.
t3.micro/t2.micro is only “free” inside a monthly hour allowance and only in a new account. An instance left running past the allowance bills normally.
So the rule for this lab is not “it is free, leave it” but:

Create → verify → stop the instance → delete the stack the same day. Treat “I will clean up later” as the failure mode. Lab 11 is designed to take four minutes, not to be a chore you postpone.

The budget alarm is your only early warning, which is why it is step 2 and not an afterthought.



# Optional - Setup your Free Azure Account 


3. Free Azure account (optional — Lab 8 only)
Create the account at azure.microsoft.com/free.
Set a Cost alert in Cost Management → Budgets ($1–5).
Do not upgrade to pay-as-you-go for this lab.
You can complete Labs 0–7 and 9–11 without Azure.
