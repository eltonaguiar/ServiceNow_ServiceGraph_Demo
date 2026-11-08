# Screenshot inventory (demo publish)

Tracks what is **in this repository** vs **missing**. Lab 7 PNGs align with the [full lab site](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/assets/img/lab07/) where noted.

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
| `assets/img/lab07/payments-lab-screenshot.png` | 7 | Wizard **Preview** (mirrored from full lab `main`) |
| `assets/img/lab07/payments-lab-view-map.png` | 7 | View Map checkpoint (redacted metadata) |
| `assets/img/lab07/business-service-payment-processing.png` | 7 | **Historical:** saved BS before Checkpoint D — **empty Business Capabilities** tab |
| `assets/img/lab07/business-capability-relationship-unsaved.png` | 7 | **Historical:** unsaved Parent trap — **not** final `Accept Customer Payments` row |

**Counts:** Lab **7** shipped PNGs: **6** (same six filenames as [full lab published set](https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/labs/07-csdm-graph/#lab7-completion-criteria)); **15** total under `assets/img/` in this demo repo.

**Provenance:** Lab 04–06 from full lab `main`; Lab 07 dropdown/map/preview from `https://eltonaguiar.github.io/ServiceNow_ServiceGraph_Lab/assets/img/lab07/`.

**Labs 0–3 / personal AWS walkthrough:** Still use existing `user-attachments` URLs in README (demo repo history).

## Missing — hands-on complete on PDI, **no fabricated images**

| Needed for | Content |
|------------|---------|
| Lab 6 step 6.3–6.7 | Sample picker, duplicate error, Comments mapping, VM Comments, Warning Log |
| Lab 7 | **Dependency Views** five-node graph (`Accept Customer Payments` → … → VM) |
| Lab 7 | Saved **`Accept Customer Payments`** capability form / Business Capabilities tab **after** Checkpoint D |
| Lab 7 | Incident **`INC0010001`** — New, saved, related lists, resolve banner |

**Transfer status:** User-visible captures exist on PDI; **ZIP / BOX transfer pending** — README documents outcomes in text only.

## Next update

When the user supplies a ZIP (or transfer succeeds), add under `assets/img/lab06/` and `assets/img/lab07/` and refresh README labels so historical PNGs are not read as the final graph or capability state.
