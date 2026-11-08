# Screenshot inventory (demo publish)

Tracks what is **in this repository** vs **missing**. Lab 7 map/dropdown PNGs match privacy-reviewed assets on the [full lab site](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/assets/img/lab07/).

## Included in repo (`main`)

| Path | Lab | Description |
|------|-----|-------------|
| `assets/img/lab04/*.png` (6 files) | 4 | pending, datacenter, aws-vpcs, vpc-transform, vpc-rerun, vpc-success |
| `assets/img/lab05/vm-discovery-source.png` | 5 | One VM, SG-AWS source |
| `assets/img/lab05/vm-identification-rule.png` | 5 | Dependent rule, Object ID |
| `assets/img/lab05/vm-dependent-relationships.png` | 5 | Relationship vocabulary |
| `assets/img/lab06/etl-vm-name-mapping.png` | 6 | Stock Name ← `object.ec2Name` |
| `assets/img/lab07/choose-method-dropdown-top.png` | 7 | Choose a Method (top of list) |
| `assets/img/lab07/choose-method-dropdown-scrolled.png` | 7 | Manual / Dynamic Service rows |
| `assets/img/lab07/payments-lab-view-map.png` | 7 | View Map checkpoint (redacted metadata) |
| `assets/img/lab07/business-service-payment-processing.png` | 7 | Saved BS; empty Business Capabilities |
| `assets/img/lab07/business-capability-relationship-unsaved.png` | 7 | Unsaved capability trap — not submitted |

**Provenance:** Lab 04–06 from full lab `main`; Lab 07 dropdown/map synced from `https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/assets/img/lab07/`.

**Labs 0–3 / personal AWS walkthrough:** Still use existing `user-attachments` URLs in README (demo repo history).

## Missing — text steps present, **no fabricated images**

| Needed for | Content |
|------------|---------|
| Lab 6 step 6.3–6.7 | Sample picker, duplicate error, Comments mapping, VM Comments, Warning Log |
| Lab 7 step 7.2 | **Provide Basic Details** form |
| Lab 7 steps 7.4–7.6 | Manual Save toast/card, **Preview** panes, **Done** banner |
| Lab 7 steps 7.8–7.9, 7.11–7.12 | Business Application, Set relationships confirmations |
| Lab 7 step 7.13 | Hierarchy **dependency views** graph |
| Lab 7 step 7.15 | Incident **New** (initial browse) |
| Lab 7 step 7.16 (Part C) | Saved incident **`INC0010001`**, related lists, resolve mandatory fields, **Resolved** banner |

**Part C (7.16):** Practice incident was **executed and resolved** on the PDI; captures were visible locally but **transfer failed** (no BOX/file payload in the publish path) — README documents fields/text only, **no fabricated PNGs**.

**Not added:** `https://raw.githubusercontent.com/eltonaguiar/poc/main/payments-lab-screenshot.png` inspected (HTTP 200) — redundant with View Map asset; no Preview-specific bytes published.

## Next update

When intermediate Lab 6 / Lab 7 PNGs arrive, add under `assets/img/lab06/` and `assets/img/lab07/` and refresh README “Screenshot missing” blocks.
