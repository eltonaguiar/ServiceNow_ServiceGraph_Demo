# Screenshot inventory (demo publish)

Tracks what is **in this repository** vs **missing**. Do not embed URLs that fail fetch (broken images on GitHub).

## Included in repo (`main`)

| Path | Lab | Description |
|------|-----|-------------|
| `assets/img/lab04/*.png` (6 files) | 4 | pending, datacenter, aws-vpcs, vpc-transform, vpc-rerun, vpc-success |
| `assets/img/lab05/vm-discovery-source.png` | 5 | One VM, SG-AWS source |
| `assets/img/lab05/vm-identification-rule.png` | 5 | Dependent rule, Object ID |
| `assets/img/lab05/vm-dependent-relationships.png` | 5 | Relationship vocabulary |
| `assets/img/lab06/etl-vm-name-mapping.png` | 6 | Stock Name ← `object.ec2Name` |
| `assets/img/lab07/choose-population-method-top.png` | 7 | Populate — Choose a Method (upper list) |
| `assets/img/lab07/choose-population-method-lower.png` | 7 | Same dialog — Manual / Dynamic Service rows |

**Provenance:** Copied from [ServiceNow_ServiceGraph_Lab](https://github.com/eltonaguiar/ServiceNow_ServiceGraph_Lab) `main` `assets/img/` (privacy-reviewed).

**Labs 0–3 / personal AWS walkthrough:** Still use existing `user-attachments` URLs in README (demo repo history).

## Missing — text steps present, **no fabricated images**

| Needed for | Content |
|------------|---------|
| Lab 6 step 6.3 | Sample import set picker (`ISET0010031` vs 0-row `ISET0010032`) |
| Lab 6 step 6.4 | Duplicate-map error dialog |
| Lab 6 step 6.5 | Edit Mapping: **Comments** ← `object.awsRegion` |
| Lab 6 step 6.6 | VM list with **Comments** = `us-east-2` after Run Integration |
| Lab 6 step 6.7 | Warning Log (NIC, storage volume, ABANDONED `cmdb_rel_ci`) |
| Lab 7 step 7.2 | **Provide Basic Details** (Name / Environment / Operational status) |
| Lab 7 after 7.3 | Manual selection, CI picker, Preview, saved service, graph |

## Next update

When PNGs are added under `assets/img/lab06/` and remaining `assets/img/lab07/`, replace “Screenshot missing” blocks in README and refresh this file.
