# Jio CareOps Network Issue & Customer Support Analytics

A Python/Pandas workflow (Google Colab) that combines customer complaints, network incident reports, recharge failures and service requests into one prioritised complaint report for the Customer Care and Network Operations teams.

> All data is fictional training data. It is not real Jio customer or network data.

**Colab notebook:** <PASTE YOUR COLAB VIEW LINK HERE>
**Explanation video:** <PASTE YOUR VIDEO LINK HERE>

## Problem
Complaints, network incidents, recharge failures and service requests are stored in separate files, so support teams cannot easily see which complaints need attention first.

## What this project does
1. Loads a CSV, an Excel file and a JSON file.
2. Cleans the data: duplicate IDs, mixed spellings, currency symbols and invalid dates. Orphan records (IDs with no matching complaint or site) are flagged and kept for review.
3. Summarises network, recharge and service data first, then joins them so that each complaint appears exactly once (4,901 complaints).
4. Flags four risk signals per complaint:
   - **Network:** open Critical / Major Outage report at the cell site, or a site marked Down, Inactive or Outage
   - **Recharge:** at least one unresolved refund
   - **Service follow-up:** an escalated request, or one over the 72-hour SLA (assumption)
   - **Repeat contact:** customer has 3 or more complaints
5. Assigns a priority: **P0** (3–4 signals), **P1** (2), **P2** (1), **P3** (0).
6. Recommends one action and owner per complaint, and builds an offline, privacy-safe AI prompt (no names, no external API call).
7. Exports reports and provides a search/filter tool (`filter_cases` plus a Colab widget).

## Key results
| Priority | Complaints | Share |
|---|---|---|
| P0 – Critical | 584 | 11.9% |
| P1 – High | 1,888 | 38.5% |
| P2 – Medium | 2,224 | 45.4% |
| P3 – Low | 205 | 4.2% |

- Busiest city: Indore (452 complaints); busiest circle: Gujarat (779, two cities combined)
- 924 complaints have an unresolved refund (about ₹341,041)
- 868 complaints have an escalated or SLA-review service request
- Automated self-assessment: **100/100**

## Files in this repository
| File | Purpose |
|---|---|
| `Jio_CareOps_Student_Starter.ipynb` | Completed notebook with code, outputs, findings and fix log |
| `jio_complaint_priority_report.csv` | Output: complaint-level priority report |
| `jio_careops_operations_summary.xlsx` | Output: priority, circle, location and owner-workload sheets |

## Input data (not included in this repository)
The notebook needs three course-provided files, packaged in a dataset ZIP:

| File | Contents |
|---|---|
| `complaints.csv` | Complaint records |
| `network_issue_reports.xlsx` | Network incident reports |
| `careops_reference.json` | Customers, cell sites, recharge failures, service requests, notes |

Download the ZIP: <https://drive.google.com/file/d/1hgqvHzcqbEDG5e_sjSXjPljwV2_u7noh/view?usp=sharing>

## How to run
1. Open the notebook in Google Colab.
2. Run the first code cell and upload `jio_careops_project_datasets.zip` when asked.
3. Choose **Runtime → Restart session**, then **Run all**.
4. The two report files are created in the Colab file panel.

## Assumptions and limitations
- `CMP001502` appears 100 times with different details; the first row is kept (99 removed).
- No cell site has a Down, Inactive or Outage status, so the network signal relies on open Critical and Major Outage reports.
- No SLA is documented, so 72 hours is an assumed threshold.
- 90.6% of complaints come from customers with 3+ complaints, so the repeat-contact signal separates cases weakly.
- Missing complaint status (452) is treated as unresolved.
- The priority rules ignore complaint status, so 169 P0 cases are already Resolved or Closed and should be reviewed separately.

## Tools
Python, Pandas, openpyxl, ipywidgets, Google Colab.
