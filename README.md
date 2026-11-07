

ServiceNow PDI



# ServiceNow_ServiceGraph_Demo

> [!WARNING]
> This is an attempt to be no cost, but it may instead be low cost. See AWS/Azure, follow the balance warnings, etc.

Following this Lab I generated with AI, specifically to teach the Service Graph Connector, and get something working for individuals who don't have SCCM / Windows Server. So if you have a typical Windows 11 Machine or maybe Linux...

**Lab:** https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/00-prerequisites/

Below is a condensed version from my actual experience in running this lab.

---

## PDI

Get your PDI at [developer.servicenow.com](https://developer.servicenow.com).

Install these, to avoid delays, it will take some time to install!
<img width="847" height="291" alt="image" src="https://github.com/user-attachments/assets/6ca540ee-67c3-4f20-8472-5b49648c8c10" />
To Install, for PDI, you can try to search for "Service Graph" under https://developer.servicenow.com/dev.do#!/manage-instance
otherwise, in your instance itself 

<img width="203" height="472" alt="image" src="https://github.com/user-attachments/assets/df353bc9-bafa-4fcf-aba2-2b32dc133b51" />
All Available Applications
<img width="911" height="132" alt="image" src="https://github.com/user-attachments/assets/236ff4a3-ba91-4d0b-81e2-9216ab6358d9" />
under search, find and then press Install. Most important one for the main lab is Service Graph Connector For AWS. However, for another lab Service Graph Connector For Azure is nice to have(Lab 8, if you choose to)
<img width="820" height="294" alt="image" src="https://github.com/user-attachments/assets/da60d7d7-fa81-46b0-8d9a-92c347862030" />




---

## Setup your AWS Account

> [!WARNING]
> Credit Card Required. Be careful, and stop the instance on the same day.

### Free AWS account (Amazon Web Services)
so starting here:
https://aws.amazon.com/pricing/
<img width="668" height="116" alt="image" src="https://github.com/user-attachments/assets/d4eb4663-7c9e-4b27-ae59-60caf8968636" />




1. Create the account at [aws.amazon.com/free](https://aws.amazon.com/free). A card is required for identity verification.
2. **Immediately** set a budget alarm. The **Settings → Billing** page (credits and days remaining) does **not** have budgets. Open the **Billing and Cost Management** console instead:
   - Direct link: [https://us-east-1.console.aws.amazon.com/cost-management](https://us-east-1.console.aws.amazon.com/cost-management) (or search for **Budgets** in the top search bar).
   - You will see something like this when you click your profile on the top right
   - <img width="283" height="433" alt="image" src="https://github.com/user-attachments/assets/87695000-bf82-4c95-97db-47a78b5f1337" />
  
   - Alternative way: <img width="1305" height="332" alt="image" src="https://github.com/user-attachments/assets/16a65d14-301f-4f1c-8c45-5fe95252e5bc" /> go to Billing & Cost Management
   - Choose **Budgets** → **Create budget** → **Use a template (simplified)**.
   - Pick **Zero spend budget** or a **Monthly cost budget** of **$1–5** with an alert at 50%.
  
I put a zero spend budget:
<img width="1029" height="312" alt="image" src="https://github.com/user-attachments/assets/603f33de-0eef-4b1f-b90c-f0e2bde6e692" />

   - Enter your email, create the budget, and **verify the email** so alerts actually send.


Email verify confirmation method (confirmation email landed in my spam folder initially)
Find email like this:
<img width="760" height="368" alt="image" src="https://github.com/user-attachments/assets/147ecdac-10db-47b8-8c01-0657269fdb7c" />
Click the Verify link:
   - <img width="560" height="294" alt="image" src="https://github.com/user-attachments/assets/feaa707f-4160-48fe-b84f-07488b054475" />

4. Choose one region and stick to it for the whole lab (for example `ca-central-1`). Multi-region imports multiply the data and the surprises.
5. Note your **12-digit account ID** (top-right account menu).



top right menu-->My Account-->AWS Management Console-->Search-->CloudFormation , it will go into your Recently Visited after
<img width="1090" height="185" alt="image" src="https://github.com/user-attachments/assets/a2ae9323-74b9-429d-be7c-72af737f433e" />
-->

   https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2
(or replace us-east-2 with whatever region you chose in Lab 0).





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

## Optional — Setup your Free Azure Account (Lab 8, if you choose to)

*(Lab 8 only)*

1. Create the account at [azure.microsoft.com/free](https://azure.microsoft.com/free).
2. Set a **Cost alert** in Cost Management → Budgets ($1–5).
3. Do **not** upgrade to pay-as-you-go for this lab.

You can complete Labs 0–7 and 9–11 without Azure.


# LAB 1 
Get the Template YAML lab-stack.yaml https://github.com/eltonaguiar/ServiceNow_ServiceGraph_Lab/blob/main/aws/lab-stack.yaml , then upload it under the Create Stack page
<img width="1772" height="796" alt="image" src="https://github.com/user-attachments/assets/0aeb4c8a-6fd5-4b23-96dd-6ed234ea9390" />
proceed to next step 
Step 2:
<img width="1698" height="751" alt="image" src="https://github.com/user-attachments/assets/68e06fa5-8d68-46aa-b4ee-e33fbfef2d36" />

Stack name: enter sgclab.
ApplicationTag: leave payments.
InstanceType: leave t3.micro.
LabOwner: replace labuser with a label such as elton.
LatestAmazonLinuxAmi: leave the existing value unchanged.
Click Next, leave the stack options at their defaults, then Next → Review and create → Submit. Wait for CREATE_COMPLETE.

Afterward, open EC2 → Instances in the same region and stop—not terminate—the lab instance. It remains discoverable while stopped. Running compute can incur charges, and storage may still cost money while stopped.


Once done , note the CREATE_COMPLETE in the left section called Stacks
<img width="376" height="276" alt="image" src="https://github.com/user-attachments/assets/ee267d97-2fb5-4e85-baae-0cfdf214a8c4" />

Before moving to Lab 2, also record the instance ID, VPC ID and region for the later inventory checks.


So the region would be found in the URL , and the rest under resources
<img width="1290" height="368" alt="image" src="https://github.com/user-attachments/assets/0650539e-a3bc-49bf-9016-b4e6fbb74cd0" />

Under Resources, copy the Physical ID beside LabInstance (i-…) and LabVpc (vpc-…). My region is us-east-2 (Ohio) , as per the URL.
Click the LabInstance Physical ID to open EC2, then select Instance state → Stop instance.

Stop the Instance 
<img width="724" height="444" alt="image" src="https://github.com/user-attachments/assets/fcbf7e18-5482-4467-a176-988dbecb855f" />
Stopping it avoids running compute charges while you’re not using it. For this inventory lab, the connector can still discover a stopped instance and its tags; you don’t need the operating system running. Stop, don’t terminate—storage can still incur charges.

So per Lab 1 you should have these documents:
<img width="601" height="278" alt="image" src="https://github.com/user-attachments/assets/d62fa693-094d-432c-8d5f-7d0fa5775c8f" />
For Region its also in the URL


Account name could be acquired by going to Profile under the top right User profile menu
<img width="258" height="404" alt="image" src="https://github.com/user-attachments/assets/15c77108-4737-4246-a92b-b65c6d67c0cb" />
<img width="1808" height="698" alt="image" src="https://github.com/user-attachments/assets/1fe78c67-7757-4e21-bb42-210c92c0c04a" />



# Lab 2 , verify apps, instaleld
<img width="1882" height="567" alt="image" src="https://github.com/user-attachments/assets/7c5d73e2-dbc3-466f-b4ff-013f73caf7ca" />
If not installed, then Install 
Service Graph Connector for AWS.

Navigate  to sys_store_app_list.do (you can type under filter navigator to get there)
https://dev296154.service-now.com/now/nav/ui/classic/params/target/sys_store_app_list.do%3Fsysparm_first_row%3D1%26sysparm_query%3DGOTOnameLIKEaws%26sysparm_query_encoded%3DGOTOnameLIKEaws%26sysparm_view%3D


<img width="1700" height="166" alt="image" src="https://github.com/user-attachments/assets/21475cfe-c336-4357-b01a-53c5466a7b5e" />
ServiceNow 
Service Graph Connector for AWS --> sn_aws_integ


All dependencies, should have been installed with it e.g. 
<img width="907" height="182" alt="image" src="https://github.com/user-attachments/assets/54e8501c-2f46-4346-b029-c7a5e9b259e1" />

Confirm IntegrationHub
Filter Navigator → Process Automation → Flow Designer. If it opens, IntegrationHub is active.
If Flow Designer is missing, the cloud REST flow cannot run — go to the fallback path.
<img width="211" height="214" alt="image" src="https://github.com/user-attachments/assets/d51bbb34-ca93-4fa7-9c66-fe2b1c0b1ea7" />

AWS-Setup>
<img width="331" height="739" alt="image" src="https://github.com/user-attachments/assets/66df954c-f397-462a-a4b1-94b90650b18d" />


Get Started
<img width="1839" height="341" alt="image" src="https://github.com/user-attachments/assets/3f2c0622-d877-49d7-abb3-5b3ffba10197" />
<img width="1907" height="897" alt="image" src="https://github.com/user-attachments/assets/48407920-afa5-4455-b2d9-a820683ab4a7" />






# Lab 3 
to get there if in Console you can use the top menu Search and type IAM, then follow the link
<img width="1894" height="152" alt="image" src="https://github.com/user-attachments/assets/cce9a5ed-f292-4f52-aef9-bfd341e55954" />

Get to IAM
<img width="1917" height="374" alt="image" src="https://github.com/user-attachments/assets/22e04e2a-8ab5-4271-af0e-103e9b681f4f" />

follow lab instructions:
<img width="946" height="421" alt="image" src="https://github.com/user-attachments/assets/bde03798-6909-4b05-ac3b-dbb057d87d6b" />

Next step --> Permission-->Create Policy-->JSON.. 
<img width="1809" height="816" alt="image" src="https://github.com/user-attachments/assets/34256538-6dd6-416d-abc3-7ada7c8e5f7f" />
paste the contents of https://github.com/eltonaguiar/ServiceNow_ServiceGraph_Lab/blob/main/aws/iam-readonly-lab-policy.json


<img width="1897" height="824" alt="image" src="https://github.com/user-attachments/assets/627450ac-776e-4702-856f-974febfca307" />
Enter these details:

Policy name: ServiceNowSGCLabReadOnly (a suggested name for this lab).
Description: Read-only AWS inventory permissions for the ServiceNow Service Graph lab.
Tags: optional—leave empty.
Click Create policy. Then return to the user-creation tab, refresh the policy list, search for ServiceNowSGCLabReadOnly, select it, and continue creating servicenow-sgc-lab without console access.

Creating the policy doesn't attach it automatically.



On the other tab , I pressed Previous-->then next, then refresh and pasted in the policy name I created (ServiceNowSGCLabReadOnly)
<img width="1133" height="508" alt="image" src="https://github.com/user-attachments/assets/953228e7-699f-4b95-9b5e-b6878c355d00" />

Thus, attaching the policy to the user (you need to check the checkbox beside the Policy, see this screenshot)

<img width="1467" height="535" alt="image" src="https://github.com/user-attachments/assets/155d4944-5f5c-413b-bbf2-209eb104d1f3" />

The correct policy is selected. Leave Set permissions boundary unchanged, then:

Click Next.
Confirm the username is servicenow-sgc-lab and the attached policy is ServiceNowSGCLabReadOnly.
Click Create user.
Open the new user → Security credentials → Create access key.
Choose Application running outside AWS, then follow the prompts.
Keep the secret access key private—don’t include it in screenshots or chat.

<img width="1434" height="565" alt="image" src="https://github.com/user-attachments/assets/d0fe6f81-24fb-4f31-9400-03874bb8b3d5" />


User Created Successfully
<img width="1912" height="406" alt="image" src="https://github.com/user-attachments/assets/8b85466c-14ba-46f0-bac5-5ed9ccb5af4a" />



next Open the new user → Security credentials → Create access key.
Choose Application running outside AWS, then follow the prompts.
Keep the secret access key private—don’t include it in screenshots or chat.
<img width="1481" height="641" alt="image" src="https://github.com/user-attachments/assets/69229e87-388c-41ac-97c9-d847d871ead3" />
Create Access Key




<img width="1539" height="737" alt="image" src="https://github.com/user-attachments/assets/9e6030e2-0859-4fc8-908f-4324cc536624" />

Create a Stack, following these steps:
Here's the exact run, in order. You're doing three of the seven instruction groups; the rest get marked off as skipped.

In ServiceNow — Guided Setup

Download the scripts task → Configure → download the CloudFormation zip to your machine → Close → mark the task complete. Unzip it and find EnableAWSConfig.yml — that's the only file you need for now.
In the AWS Console (your lab account, your one region)

CloudFormation → Create stack → With new resources → Upload a template file → choose EnableAWSConfig.yml.
Stack name: sg-aws-config. Leave parameters at defaults unless one is clearly required (the template creates its own S3 bucket for Config delivery). If it asks you to acknowledge IAM resource creation, tick it — the stack creates the role AWS Config uses to record and deliver.
Submit and wait for CREATE_COMPLETE (a few minutes).
Sanity check: AWS Config → Dashboard/Settings — the recorder should show as On. That recorder is the connector's data source; without it, imports return nothing.
Back in ServiceNow — Guided Setup

AWS Setup Instructions task → open it, confirm the recorder group is the one you've done, and mark it complete. Aggregator, SSM, SendCommand, key rotation, and email notification are the skippable groups — single account, no deep discovery, lab account.
Move to the connection/configuration section of guided setup and confirm it points at what you built in Lab 3: your 12-digit account ID, the one region, and the credential record for servicenow-sgc-lab. Leave the aggregator field blank and skip any SSM/deep-discovery role step — both are the optional paths.
Run the Diagnostic Tool at the end of guided setup. Fix only what it flags.
Click Test connection / Validate — green here is your guided-setup finish line. Leave the scheduled import jobs alone; running the first import is Lab 4.
One thing to check before Lab 4, so it doesn't fail on you: your Lab 3 policy grants EC2/S3/SSM/Organizations/IAM/Tag reads, but the connector's core data call is config:ListDiscoveredResources — it reads hardware out of AWS Config, not straight from EC2. Compare your policy against the prerequisites policy that ships in the scripts download (that's the authoritative minimum for your connector version) and add just the missing config: read actions — ListDiscoveredResources and its Describe/Get companions. Don't swap in ReadOnlyAccess wholesale; three or four lines is the whole fix.

Cost check: the only meter running is AWS Config recording (~$0.003 per recorded item) plus pennies of S3 storage. When the lab series is finished, delete the sg-aws-config stack and empty its bucket so the recorder doesn't keep billing in the background.
<img width="1862" height="453" alt="image" src="https://github.com/user-attachments/assets/01854d19-a9f3-4a1b-bbf0-23bb28d22a76" />

so 2 stacks, the first one from earlier and this one
<img width="359" height="368" alt="image" src="https://github.com/user-attachments/assets/21c914ad-279c-46a7-bd61-7b5423477f79" />
