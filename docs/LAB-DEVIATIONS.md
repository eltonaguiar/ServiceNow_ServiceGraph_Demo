# Private amendments — demo PDI vs published full lab

**Status:** Demo-repo companion doc (PDI-specific). **Does not** change the [full lab site](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/) unless separately published there.

This file records what actually happened on the walkthrough PDI (Ohio `us-east-2`) so the public **full lab** pages can be corrected later without rewriting the light demo README into a curriculum.

---

## Progress index (demo repo walkthrough)

| Lab | Published URL | Demo progress | Labs 8–11 |
|-----|---------------|---------------|-----------|
| 0 | [Prerequisites](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/00-prerequisites/) | Completed (PDI + AWS account + budget) | — |
| 1 | [AWS inventory](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/01-aws-inventory/) | Stack `sgclab`; **one stopped** `sgclab-app-01` `t3.micro` | — |
| 2 | [SGC install](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/02-sgc-install/) | Connector + Guided Setup started | — |
| 3 | [IAM / connection](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/03-iam-connection/) | Shortened path in demo README; authoritative steps on lab site | — |
| 4 | [Run import](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/04-run-import/) | Standalone orders **100→180** (see demo README); partial graph acknowledged | — |
| 5 | [IRE](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/05-ire/) | **Observational minimum** (list + rule read-only); relationship debug **deferred** | — |
| 6 | [ETL mappings](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/06-etl-mappings/) | **Hands-on mapping exercise** (see §6 below); **not** Part A read-only only | — |
| 7 | [CSDM / service graph](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/07-csdm-graph/) | **In progress** — manual map checkpoint done; **Business Application** / offering layer pending; Preview relationships empty; no DependsOn verified | **Not started** |
| 8–11 | [08](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/08-multisource-azure/) … [11](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/11-cleanup/) | **Not attempted** | **Not started** |

---

## Lab 6 — published page vs actual PDI route

**Published intent ([Lab 6](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/06-etl-mappings/)):** Part A = **read-only** navigation; optional Part B = clone/customize on own PDI.

**Actual demo choice:** Hands-on field mapping **before** Lab 7; did **not** stop at read-only minimum.

### UI path (verified)

1. **All** → **IntegrationHub ETL** → **Configuration** → **IntegrationHub ETL** (release wording may vary).
2. **CMDB Application: SG-AWS** → definition **`SG-AWS-EC2-Instance`** (ETL definition name — not schedule `SG-AWS-EC2`).
3. **Map Data to CMDB** → step 3 **Select CMDB Classes** → **Virtual Machine Instance** card → **Edit Mapping**.

### Sample import set (deviation)

| Observation | Detail |
|-------------|--------|
| **Add Attribute greyed** until a populated sample is selected | Expected until sample exists |
| **`ISET0010032`** | **0 rows** — not usable |
| **`ISET0010031`** | **1 row**, **Auto-pull OFF** → **Edit Mapping** enabled |

### Duplicate-map route abandoned (deviation from “clone Part B”)

| Attempt | Result |
|---------|--------|
| Duplicate original map using same source | Exact error: *“This map cannot be used as a copy source for the selected data source.”* |
| Official duplicate | Requires a **separate unused** preexisting source with same schema — route abandoned |
| New replay source | Considered, **not created** |
| **Chosen path** | **Direct edit of original map** on disposable PDI — **lab-only, not production** |

**Lesson for full lab doc:** Do not teach blind duplicate of `SG-AWS` loader maps. Copying/duplicating can tie to `data_source_id` and update **`last_run_datetime` on the original** when misused.

### Mapping applied (temporary lab edit)

| Target | Transform |
|--------|-----------|
| Existing string field **Comments** on `cmdb_ci_vm_instance` (and other classes on same map) | Source **`object.awsRegion`** |
| **Name** (baseline) | Still **`object.ec2Name`** per stock map — see published screenshot |

**One map → multiple CMDB classes:** The EC2 instance ETL map targets **Virtual Machine Instance** and related classes in one definition — unlike a classic single-target transform mental model.

**Before:** VM **Comments** blank. **After Run Integration:** `sgclab-app-01` **Comments** = `us-east-2` (confirms write path). **No duplicate VM.**

### Test and Rollback — actual results

**Mark as Complete** → step 4 **Test and Rollback** → **Integration Results** → **Run Integration** (real **IRE writes**, not simulation).

| Metric | Value |
|--------|------:|
| Classes | 15 |
| New | 0 |
| Updated | 2 (VM + OS template class) |
| Partial | 1 |
| Incomplete | 0 |

**Error log:** empty. **Warning log:** 2 × `MISSING_DEPENDENCY` (`cmdb_ci_nic`, `cmdb_ci_storage_volume`); 3 × `ABANDONED` `cmdb_rel_ci`; shared partial payload.

**Relationship rules (not changed):**

- NIC: needs **Hosted on** logical datacenter **or** **Contained by** VM; payload **Use End Point To** did not satisfy listed rules.
- Storage volume: **Contains** from VM did not satisfy Computer / logical-datacenter rules shown in UI.

**Unresolved:** Whether narrowing AWS Config recording fixes these — **not asserted** here. Condition `false AND not-empty` did pass; relationship was present in payload. **User deferred** relationship debugging; **no connector rule edits.**

### Cleanup pending (explicit)

- Temporary **Comments** mapping and written values **not confirmed reverted**.
- Removing mapping alone **does not erase** CMDB data already written.

---

## Lab 7 — start only (deviations)

**Published corrections to represent later:**

1. **Application service** in CSDM is already a **service instance** (`cmdb_ci_service_auto` / Application Service). Do **not** invent an extra generic `cmdb_ci_service_instance` layer for this wizard.
2. **Navigator:** `cmdb_ci_service_auto.list` — UI title **Service Instances**. Pre-existing **ServiceNow Event Management** service left untouched.
3. **Wizard:** **New** → **Create an Application Service** — steps **Provide Basic Details** / **Populate** / **Preview**.
4. **User state:** Last screenshot showed **blank wizard**. Advisable values discussed: name **Payments** (lab tag only — **no real Payments app** installed), description noting learning link, **Non-Operational**, inspect **Environment** choices.
5. **No evidence:** Fields entered, service saved, VM attached, or dependency/graph screenshot.
6. **Population:** Manual path references `cmdb_ci_service_discovered` — **picker not verified** on this PDI.
7. **ELI5 framing:** A **business capability you depend on** (here the lab label “payments”) **depends on** the rented cloud computer records — **asserted dependency for learning**, not discovered from a running application.

**Do not imply:** Lab 7 complete, Payments service graph proven, or Labs 8–12 done.

---

## Suggested full-lab page edits (future batch)

| Page | Suggested change |
|------|------------------|
| [06-etl-mappings](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/06-etl-mappings/) | Document **ISET0010031** vs empty sample; duplicate-map error; **direct-edit disposable PDI** fork; **Run Integration** counters; warning log partials |
| [07-csdm-graph](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/07-csdm-graph/) | Clarify **service instance** table; warn against duplicate `cmdb_ci_service_instance` chain; separate **wizard start** from **saved service** evidence |

---

## Class / table corrections (verified)

| Import order | Schedule (suffix may appear on some lists) | CMDB table |
|-------------|---------------------------------------------|------------|
| 100 | Service Account | `cmdb_ci_cloud_service_account` |
| 110 | Datacenters | `cmdb_ci_aws_datacenter` |
| 120 | VPC | `cmdb_ci_network` (**not** `cmdb_ci_vpc`) |
| 130 | Subnets | `cmdb_ci_cloud_subnet` |
| 140 | Network Interface | Optional — **0 Config rows** in lab account when Config records only 4 types |
| 150 | Security Group | `cmdb_ci_compute_security_group` |
| 180 | EC2 | `cmdb_ci_vm_instance` (**not** `cmdb_ci_ec2_instance`, **not** `cmdb_ci_win_server`) |

**Discovery source on VM:** `SG-AWS`. Stopped instance → **no guest OS discovery**; cloud VM row ≠ Windows Server class.
