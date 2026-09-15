---
node_id: ref-pdf-field-inventory
type: reference
title: Complete discovered AcroForm inventory
created: 2026-09-14
updated: 2026-09-14
status: active
category: reference
tags: [pdf, inventory]
summary: Every discovered field and widget from the unchanged supplied templates.
---

# Appendix - discovered AcroForm field inventory

Read-only inspection; 189 fields / 189 widgets total. Labels below are interpretations of visible page text, not PDF tooltips. Exact PDF names are authoritative. Each field has exactly one widget and no parent/kids in these files. Object references identify this template revision only; runtime IDs must be derived again when loading a copy.

All buttons have on `/1`, off `/Off`, and `/AP/N` keys only `/1`. Their `/DV` is absent. `None` means the PDF key was absent; `blank` is an empty text value; `space` is one literal space. Text fields have no declared `/MaxLen`. All widgets have annotation flags `/F=4`. Field flags `/Ff=2` mean required; `/Ff=0` means no field flags. All page references are physical 1-based PDF pages, including consent.

Rectangles are PDF points `[left, bottom, right, top]` on a 612 x 792 point unrotated page. Convert through the viewer viewport, never by a hardcoded scale.

[Findings and semantics](pdf-findings.md) | [Mapping design](../specs/2026-09-14-data-mapping-sync.md)

## TWS_Rollover Attestation_(FILLABLE)_10.31.2022.pdf

SHA-256: `1cf50ec1d712c6063b7cc212689937dedfb724573312ef7a1719a68c4ad03534`

3 pages; 14 fields.

| Exact field name | Visible meaning / proposed label | Type | Ff | Page | Field / widget object | V / AS | Rectangle |
|---|---|---|---:|---:|---|---|---|
| `Signature2_es_:signer1:signature` | Client consent signature (downstream) | `/Tx` | 2 | 1 | `253 0 R` / `253 0 R` | `None` / `None` | 71.227, 285.348, 181.742, 307.348 |
| `Signature16_es_:signer:signature` | Retirement investor signature (downstream) | `/Tx` | 2 | 3 | `302 0 R` / `302 0 R` | `None` / `None` | 138.904, 336.378, 561.056, 359.807 |
| `Date_es_:signer:date` | Client signing date (downstream) | `/Tx` | 2 | 3 | `292 0 R` / `292 0 R` | `None` / `None` | 139.800, 291.736, 289.800, 313.736 |
| `text_01_es_:prefill` | Individual name | `/Tx` | 0 | 2 | `84 0 R` / `84 0 R` | `blank` / `None` | 234.716, 665.930, 558.703, 682.345 |
| `text_02_es_:prefill` | Plan sponsor/employer name | `/Tx` | 0 | 2 | `83 0 R` / `83 0 R` | `blank` / `None` | 234.716, 649.310, 558.703, 665.724 |
| `text_03_es_:prefill` | Current plan name | `/Tx` | 0 | 2 | `80 0 R` / `80 0 R` | `blank` / `None` | 234.716, 632.558, 558.703, 648.972 |
| `text_04_es_:prefill` | Plan sponsor contact | `/Tx` | 0 | 2 | `91 0 R` / `91 0 R` | `blank` / `None` | 234.716, 615.872, 558.703, 632.286 |
| `CheckBox_01_es_:prefill` | Employed and eligible for in-service distribution | `/Btn` | 0 | 2 | `87 0 R` / `87 0 R` | `None` / `/Off` | 60.539, 148.253, 69.934, 157.669 |
| `CheckBox_02_es_:prefill` | Leaving plan sponsor employment | `/Btn` | 0 | 2 | `93 0 R` / `93 0 R` | `None` / `/Off` | 60.539, 116.107, 69.934, 125.523 |
| `Date_04_af_date_es_:prefill` | Employment departure date | `/Tx` | 0 | 2 | `89 0 R` / `89 0 R` | `blank` / `None` | 194.099, 101.725, 322.296, 113.453 |
| `CheckBox_03_es_:prefill` | Other employment status | `/Btn` | 0 | 2 | `81 0 R` / `81 0 R` | `/Off` / `/Off` | 60.539, 85.493, 69.934, 94.909 |
| `text_05_es_:prefill` | Other employment description | `/Tx` | 0 | 2 | `79 0 R` / `79 0 R` | `blank` / `None` | 185.598, 84.146, 553.336, 98.371 |
| `CheckBox_04_es_:prefill` | Recommendation disclosure applies | `/Btn` | 0 | 3 | `99 0 R` / `99 0 R` | `/1` / `/1` | 88.832, 537.919, 98.226, 547.335 |
| `text_06_es_:prefill` | Printed investor name | `/Tx` | 0 | 3 | `96 0 R` / `96 0 R` | `blank` / `None` | 140.316, 314.304, 560.478, 335.974 |

## TWS_Intake_Recommendation_Attestation_Form_(FILLABLE)_10.31.2022.pdf

SHA-256: `2033f25c00beee6c5c152507b3a0ce45df826c0507b3b7cf1e81afee3e0cb6e3`

7 pages; 175 fields.

| Exact field name | Visible meaning / proposed label | Type | Ff | Page | Field / widget object | V / AS | Rectangle |
|---|---|---|---:|---:|---|---|---|
| `text_01` | Retirement investor name | `/Tx` | 0 | 2 | `1533 0 R` / `1533 0 R` | `space` / `None` | 227.704, 663.772, 575.963, 690.615 |
| `CheckBox_01` | 401(k) | `/Btn` | 0 | 2 | `1534 0 R` / `1534 0 R` | `None` / `/Off` | 233.259, 652.132, 242.654, 661.548 |
| `CheckBox_02` | 403(b) ERISA | `/Btn` | 0 | 2 | `1536 0 R` / `1536 0 R` | `None` / `/Off` | 278.818, 652.132, 288.212, 661.548 |
| `CheckBox_03` | 403(b) non-ERISA | `/Btn` | 0 | 2 | `1538 0 R` / `1538 0 R` | `None` / `/Off` | 353.270, 652.132, 362.664, 661.548 |
| `CheckBox_04` | 457 | `/Btn` | 0 | 2 | `1540 0 R` / `1540 0 R` | `/Off` / `/Off` | 449.796, 652.132, 459.190, 661.548 |
| `CheckBox_05` | Pension/defined benefit | `/Btn` | 0 | 2 | `1542 0 R` / `1542 0 R` | `None` / `/Off` | 483.912, 652.132, 493.307, 661.548 |
| `CheckBox_06` | Other employer-sponsored plan | `/Btn` | 0 | 2 | `1544 0 R` / `1544 0 R` | `None` / `/Off` | 233.259, 638.501, 242.654, 647.917 |
| `text_02` | Other employer-sponsored plan type | `/Tx` | 0 | 2 | `1546 0 R` / `1546 0 R` | `None` / `None` | 390.458, 637.352, 462.199, 650.995 |
| `CheckBox_07` | IRA | `/Btn` | 0 | 2 | `1547 0 R` / `1547 0 R` | `/1` / `/1` | 483.299, 638.774, 492.694, 648.190 |
| `CheckBox_08` | Sponsor N/A | `/Btn` | 0 | 2 | `1549 0 R` / `1549 0 R` | `None` / `/Off` | 233.259, 613.931, 242.654, 623.347 |
| `text_03` | Plan sponsor/employer name | `/Tx` | 0 | 2 | `1551 0 R` / `1551 0 R` | `None` / `None` | 265.203, 613.002, 468.593, 626.644 |
| `CheckBox_09` | Current plan N/A | `/Btn` | 0 | 2 | `1552 0 R` / `1552 0 R` | `None` / `/Off` | 233.259, 586.471, 242.654, 595.887 |
| `text_04` | Current plan name | `/Tx` | 0 | 2 | `1554 0 R` / `1554 0 R` | `None` / `None` | 265.203, 585.236, 468.593, 598.878 |
| `CheckBox_10` | Currently employed by plan sponsor | `/Btn` | 0 | 2 | `1555 0 R` / `1555 0 R` | `/Off` / `/Off` | 41.932, 543.470, 51.327, 552.886 |
| `CheckBox_11` | Planning retirement | `/Btn` | 0 | 2 | `1557 0 R` / `1557 0 R` | `None` / `/Off` | 54.217, 529.871, 63.611, 539.287 |
| `CheckBox_12` | Not planning retirement/departure | `/Btn` | 0 | 2 | `1560 0 R` / `1560 0 R` | `None` / `/Off` | 54.217, 516.502, 63.611, 525.918 |
| `CheckBox_13` | No longer employed by sponsor | `/Btn` | 0 | 2 | `1562 0 R` / `1562 0 R` | `None` / `/Off` | 41.932, 502.411, 51.327, 511.827 |
| `CheckBox_14` | Retired | `/Btn` | 0 | 2 | `1564 0 R` / `1564 0 R` | `None` / `/Off` | 54.217, 488.944, 63.611, 498.360 |
| `CheckBox_15` | Employed with new employer | `/Btn` | 0 | 2 | `1566 0 R` / `1566 0 R` | `None` / `/Off` | 54.217, 475.707, 63.611, 485.123 |
| `CheckBox_16` | Not currently employed | `/Btn` | 0 | 2 | `1568 0 R` / `1568 0 R` | `None` / `/Off` | 54.217, 462.240, 63.611, 471.656 |
| `CheckBox_17` | Quarterly statements: Provided | `/Btn` | 0 | 2 | `1570 0 R` / `1570 0 R` | `/1` / `/1` | 450.899, 364.020, 460.293, 373.436 |
| `CheckBox_18` | Quarterly statements: Not Provided | `/Btn` | 0 | 2 | `1572 0 R` / `1572 0 R` | `None` / `/Off` | 527.409, 364.020, 536.804, 373.436 |
| `CheckBox_19` | Fee disclosure: Provided | `/Btn` | 0 | 2 | `1574 0 R` / `1574 0 R` | `/Off` / `/Off` | 450.899, 344.005, 460.293, 353.421 |
| `CheckBox_20` | Fee disclosure: Not Provided | `/Btn` | 0 | 2 | `1576 0 R` / `1576 0 R` | `None` / `/Off` | 527.409, 344.005, 536.804, 353.421 |
| `CheckBox_21` | Plan description: Provided | `/Btn` | 0 | 2 | `1578 0 R` / `1578 0 R` | `/Off` / `/Off` | 450.899, 324.210, 460.293, 333.626 |
| `CheckBox_22` | Plan description: Not Provided | `/Btn` | 0 | 2 | `1580 0 R` / `1580 0 R` | `None` / `/Off` | 527.409, 324.210, 536.804, 333.626 |
| `CheckBox_23` | Current plan alignment: High | `/Btn` | 0 | 2 | `1582 0 R` / `1582 0 R` | `None` / `/Off` | 40.282, 199.786, 49.676, 209.202 |
| `CheckBox_26` | New employer alignment: High | `/Btn` | 0 | 2 | `1584 0 R` / `1584 0 R` | `None` / `/Off` | 218.180, 199.786, 227.574, 209.202 |
| `CheckBox_29` | IRA/new account alignment: High | `/Btn` | 0 | 2 | `1586 0 R` / `1586 0 R` | `None` / `/Off` | 399.231, 199.786, 408.625, 209.202 |
| `CheckBox_24` | Current plan alignment: Medium | `/Btn` | 0 | 2 | `1588 0 R` / `1588 0 R` | `None` / `/Off` | 40.282, 185.794, 49.676, 195.210 |
| `CheckBox_27` | New employer alignment: Medium | `/Btn` | 0 | 2 | `1590 0 R` / `1590 0 R` | `None` / `/Off` | 218.180, 185.794, 227.574, 195.210 |
| `CheckBox_30` | IRA/new account alignment: Medium | `/Btn` | 0 | 2 | `1592 0 R` / `1592 0 R` | `None` / `/Off` | 399.231, 185.794, 408.625, 195.210 |
| `CheckBox_25` | Current plan alignment: Low | `/Btn` | 0 | 2 | `1594 0 R` / `1594 0 R` | `None` / `/Off` | 40.282, 171.637, 49.676, 181.053 |
| `CheckBox_28` | New employer alignment: Low | `/Btn` | 0 | 2 | `1596 0 R` / `1596 0 R` | `None` / `/Off` | 218.180, 171.637, 227.574, 181.053 |
| `CheckBox_31` | IRA/new account alignment: Low | `/Btn` | 0 | 2 | `1598 0 R` / `1598 0 R` | `None` / `/Off` | 399.231, 171.637, 408.625, 181.053 |
| `CheckBox_32` | Monitoring: Need High | `/Btn` | 0 | 3 | `483 0 R` / `483 0 R` | `None` / `/Off` | 280.112, 678.428, 289.506, 687.844 |
| `CheckBox_33` | Monitoring: Need Medium | `/Btn` | 0 | 3 | `484 0 R` / `484 0 R` | `None` / `/Off` | 280.112, 664.501, 289.506, 673.917 |
| `CheckBox_36` | Monitoring: Current Yes | `/Btn` | 0 | 3 | `489 0 R` / `489 0 R` | `None` / `/Off` | 356.141, 664.501, 365.536, 673.917 |
| `CheckBox_38` | Monitoring: New employer Yes | `/Btn` | 0 | 3 | `488 0 R` / `488 0 R` | `None` / `/Off` | 430.243, 664.501, 439.638, 673.917 |
| `CheckBox_40` | Monitoring: IRA/new Yes | `/Btn` | 0 | 3 | `446 0 R` / `446 0 R` | `None` / `/Off` | 504.784, 664.501, 514.178, 673.917 |
| `CheckBox_34` | Monitoring: Need Low | `/Btn` | 0 | 3 | `449 0 R` / `449 0 R` | `None` / `/Off` | 280.112, 650.574, 289.506, 659.990 |
| `CheckBox_37` | Monitoring: Current No | `/Btn` | 0 | 3 | `445 0 R` / `445 0 R` | `None` / `/Off` | 356.141, 650.574, 365.536, 659.990 |
| `CheckBox_39` | Monitoring: New employer No | `/Btn` | 0 | 3 | `454 0 R` / `454 0 R` | `None` / `/Off` | 430.243, 650.574, 439.638, 659.990 |
| `CheckBox_41` | Monitoring: IRA/new No | `/Btn` | 0 | 3 | `456 0 R` / `456 0 R` | `None` / `/Off` | 504.784, 650.574, 514.178, 659.990 |
| `CheckBox_35` | Monitoring: Need None | `/Btn` | 0 | 3 | `462 0 R` / `462 0 R` | `/Off` / `/Off` | 280.112, 636.472, 289.506, 645.888 |
| `CheckBox_32a` | Asset allocation: Need High | `/Btn` | 0 | 3 | `457 0 R` / `457 0 R` | `None` / `/Off` | 280.112, 622.020, 289.506, 631.436 |
| `CheckBox_33a` | Asset allocation: Need Medium | `/Btn` | 0 | 3 | `452 0 R` / `452 0 R` | `None` / `/Off` | 280.112, 608.093, 289.506, 617.509 |
| `CheckBox_36a` | Asset allocation: Current Yes | `/Btn` | 0 | 3 | `461 0 R` / `461 0 R` | `None` / `/Off` | 356.141, 608.093, 365.536, 617.509 |
| `CheckBox_38a` | Asset allocation: New employer Yes | `/Btn` | 0 | 3 | `472 0 R` / `472 0 R` | `None` / `/Off` | 430.243, 608.093, 439.638, 617.509 |
| `CheckBox_40a` | Asset allocation: IRA/new Yes | `/Btn` | 0 | 3 | `467 0 R` / `467 0 R` | `None` / `/Off` | 504.784, 608.093, 514.178, 617.509 |
| `CheckBox_34a` | Asset allocation: Need Low | `/Btn` | 0 | 3 | `468 0 R` / `468 0 R` | `None` / `/Off` | 280.112, 594.166, 289.506, 603.582 |
| `CheckBox_37a` | Asset allocation: Current No | `/Btn` | 0 | 3 | `464 0 R` / `464 0 R` | `None` / `/Off` | 356.141, 594.166, 365.536, 603.582 |
| `CheckBox_39a` | Asset allocation: New employer No | `/Btn` | 0 | 3 | `466 0 R` / `466 0 R` | `None` / `/Off` | 430.243, 594.166, 439.638, 603.582 |
| `CheckBox_41a` | Asset allocation: IRA/new No | `/Btn` | 0 | 3 | `470 0 R` / `470 0 R` | `None` / `/Off` | 504.784, 594.166, 514.178, 603.582 |
| `CheckBox_35a` | Asset allocation: Need None | `/Btn` | 0 | 3 | `475 0 R` / `475 0 R` | `/Off` / `/Off` | 280.112, 580.063, 289.506, 589.479 |
| `CheckBox_32b` | Ongoing advice: Need High | `/Btn` | 0 | 3 | `479 0 R` / `479 0 R` | `None` / `/Off` | 280.112, 565.173, 289.506, 574.589 |
| `CheckBox_33b` | Ongoing advice: Need Medium | `/Btn` | 0 | 3 | `481 0 R` / `481 0 R` | `None` / `/Off` | 280.112, 551.246, 289.506, 560.662 |
| `CheckBox_36b` | Ongoing advice: Current Yes | `/Btn` | 0 | 3 | `482 0 R` / `482 0 R` | `None` / `/Off` | 356.141, 551.246, 365.536, 560.662 |
| `CheckBox_38b` | Ongoing advice: New employer Yes | `/Btn` | 0 | 3 | `414 0 R` / `414 0 R` | `None` / `/Off` | 430.243, 551.246, 439.638, 560.662 |
| `CheckBox_40b` | Ongoing advice: IRA/new Yes | `/Btn` | 0 | 3 | `417 0 R` / `417 0 R` | `None` / `/Off` | 504.784, 551.246, 514.178, 560.662 |
| `CheckBox_34b` | Ongoing advice: Need Low | `/Btn` | 0 | 3 | `418 0 R` / `418 0 R` | `None` / `/Off` | 280.112, 537.319, 289.506, 546.735 |
| `CheckBox_37b` | Ongoing advice: Current No | `/Btn` | 0 | 3 | `416 0 R` / `416 0 R` | `None` / `/Off` | 356.141, 537.319, 365.536, 546.735 |
| `CheckBox_39b` | Ongoing advice: New employer No | `/Btn` | 0 | 3 | `422 0 R` / `422 0 R` | `None` / `/Off` | 430.243, 537.319, 439.638, 546.735 |
| `CheckBox_41b` | Ongoing advice: IRA/new No | `/Btn` | 0 | 3 | `420 0 R` / `420 0 R` | `None` / `/Off` | 504.784, 537.319, 514.178, 546.735 |
| `CheckBox_35b` | Ongoing advice: Need None | `/Btn` | 0 | 3 | `424 0 R` / `424 0 R` | `/Off` / `/Off` | 280.112, 523.217, 289.506, 532.633 |
| `CheckBox_32c` | Discretionary management: Need High | `/Btn` | 0 | 3 | `425 0 R` / `425 0 R` | `None` / `/Off` | 280.112, 508.677, 289.506, 518.093 |
| `CheckBox_33c` | Discretionary management: Need Medium | `/Btn` | 0 | 3 | `427 0 R` / `427 0 R` | `None` / `/Off` | 280.112, 494.750, 289.506, 504.166 |
| `CheckBox_36c` | Discretionary management: Current Yes | `/Btn` | 0 | 3 | `430 0 R` / `430 0 R` | `None` / `/Off` | 356.141, 494.750, 365.536, 504.166 |
| `CheckBox_38c` | Discretionary management: New employer Yes | `/Btn` | 0 | 3 | `432 0 R` / `432 0 R` | `None` / `/Off` | 430.243, 494.750, 439.638, 504.166 |
| `CheckBox_40c` | Discretionary management: IRA/new Yes | `/Btn` | 0 | 3 | `435 0 R` / `435 0 R` | `None` / `/Off` | 504.784, 494.750, 514.178, 504.166 |
| `CheckBox_34c` | Discretionary management: Need Low | `/Btn` | 0 | 3 | `437 0 R` / `437 0 R` | `None` / `/Off` | 280.112, 480.823, 289.506, 490.239 |
| `CheckBox_37c` | Discretionary management: Current No | `/Btn` | 0 | 3 | `439 0 R` / `439 0 R` | `None` / `/Off` | 356.141, 480.823, 365.536, 490.239 |
| `CheckBox_39c` | Discretionary management: New employer No | `/Btn` | 0 | 3 | `438 0 R` / `438 0 R` | `None` / `/Off` | 430.243, 480.823, 439.638, 490.239 |
| `CheckBox_41c` | Discretionary management: IRA/new No | `/Btn` | 0 | 3 | `444 0 R` / `444 0 R` | `None` / `/Off` | 504.784, 480.823, 514.178, 490.239 |
| `CheckBox_35c` | Discretionary management: Need None | `/Btn` | 0 | 3 | `443 0 R` / `443 0 R` | `/Off` / `/Off` | 280.112, 466.720, 289.506, 476.136 |
| `CheckBox_32d` | Retirement/financial planning: Need High | `/Btn` | 0 | 3 | `405 0 R` / `405 0 R` | `None` / `/Off` | 280.112, 452.268, 289.506, 461.684 |
| `CheckBox_33d` | Retirement/financial planning: Need Medium | `/Btn` | 0 | 3 | `407 0 R` / `407 0 R` | `None` / `/Off` | 280.112, 438.341, 289.506, 447.757 |
| `CheckBox_36d` | Retirement/financial planning: Current Yes | `/Btn` | 0 | 3 | `404 0 R` / `404 0 R` | `None` / `/Off` | 356.141, 440.005, 365.536, 449.421 |
| `CheckBox_38d` | Retirement/financial planning: New employer Yes | `/Btn` | 0 | 3 | `547 0 R` / `547 0 R` | `None` / `/Off` | 430.243, 440.005, 439.638, 449.421 |
| `CheckBox_40d` | Retirement/financial planning: IRA/new Yes | `/Btn` | 0 | 3 | `548 0 R` / `548 0 R` | `None` / `/Off` | 504.784, 439.917, 514.178, 449.333 |
| `CheckBox_34d` | Retirement/financial planning: Need Low | `/Btn` | 0 | 3 | `549 0 R` / `549 0 R` | `None` / `/Off` | 280.112, 424.414, 289.506, 433.830 |
| `CheckBox_37d` | Retirement/financial planning: Current No | `/Btn` | 0 | 3 | `545 0 R` / `545 0 R` | `None` / `/Off` | 356.141, 426.078, 365.536, 435.494 |
| `CheckBox_39d` | Retirement/financial planning: New employer No | `/Btn` | 0 | 3 | `552 0 R` / `552 0 R` | `None` / `/Off` | 430.243, 426.078, 439.638, 435.494 |
| `CheckBox_41d` | Retirement/financial planning: IRA/new No | `/Btn` | 0 | 3 | `553 0 R` / `553 0 R` | `None` / `/Off` | 504.784, 425.815, 514.178, 435.231 |
| `CheckBox_35d` | Retirement/financial planning: Need None | `/Btn` | 0 | 3 | `556 0 R` / `556 0 R` | `/Off` / `/Off` | 280.112, 410.312, 289.506, 419.728 |
| `CheckBox_42` | Tax: Need High | `/Btn` | 0 | 3 | `558 0 R` / `558 0 R` | `/Off` / `/Off` | 355.341, 322.208, 364.735, 331.624 |
| `CheckBox_43` | Tax: Need Medium | `/Btn` | 0 | 3 | `521 0 R` / `521 0 R` | `None` / `/Off` | 355.341, 308.544, 364.735, 317.960 |
| `CheckBox_46` | Tax: Best current | `/Btn` | 0 | 3 | `522 0 R` / `522 0 R` | `None` / `/Off` | 428.129, 315.201, 437.523, 324.617 |
| `CheckBox_44` | Tax: Need Low | `/Btn` | 0 | 3 | `524 0 R` / `524 0 R` | `None` / `/Off` | 355.341, 294.486, 364.735, 303.901 |
| `CheckBox_47` | Tax: Best new employer | `/Btn` | 0 | 3 | `526 0 R` / `526 0 R` | `None` / `/Off` | 428.129, 301.405, 437.523, 310.822 |
| `CheckBox_45` | Tax: Need None | `/Btn` | 0 | 3 | `528 0 R` / `528 0 R` | `None` / `/Off` | 355.341, 280.253, 364.735, 289.668 |
| `CheckBox_48` | Tax: Best IRA/new | `/Btn` | 0 | 3 | `532 0 R` / `532 0 R` | `None` / `/Off` | 428.129, 287.478, 437.523, 296.895 |
| `CheckBox_42a` | Beneficiary: Need High | `/Btn` | 0 | 3 | `530 0 R` / `530 0 R` | `/Off` / `/Off` | 355.341, 254.807, 364.735, 264.224 |
| `CheckBox_43a` | Beneficiary: Need Medium | `/Btn` | 0 | 3 | `535 0 R` / `535 0 R` | `None` / `/Off` | 355.341, 241.142, 364.735, 250.559 |
| `CheckBox_46a` | Beneficiary: Best current | `/Btn` | 0 | 3 | `536 0 R` / `536 0 R` | `None` / `/Off` | 428.129, 247.799, 437.523, 257.216 |
| `CheckBox_44a` | Beneficiary: Need Low | `/Btn` | 0 | 3 | `538 0 R` / `538 0 R` | `None` / `/Off` | 355.341, 227.084, 364.735, 236.501 |
| `CheckBox_47a` | Beneficiary: Best new employer | `/Btn` | 0 | 3 | `542 0 R` / `542 0 R` | `None` / `/Off` | 428.129, 234.004, 437.523, 243.421 |
| `CheckBox_45a` | Beneficiary: Need None | `/Btn` | 0 | 3 | `540 0 R` / `540 0 R` | `None` / `/Off` | 355.341, 212.850, 364.735, 222.267 |
| `CheckBox_48a` | Beneficiary: Best IRA/new | `/Btn` | 0 | 3 | `490 0 R` / `490 0 R` | `None` / `/Off` | 428.129, 220.077, 437.523, 229.493 |
| `CheckBox_42b` | Guarantees: Need High | `/Btn` | 0 | 3 | `491 0 R` / `491 0 R` | `/Off` / `/Off` | 355.603, 187.318, 364.997, 196.734 |
| `CheckBox_43b` | Guarantees: Need Medium | `/Btn` | 0 | 3 | `497 0 R` / `497 0 R` | `None` / `/Off` | 355.603, 173.653, 364.997, 183.069 |
| `CheckBox_46b` | Guarantees: Best current | `/Btn` | 0 | 3 | `495 0 R` / `495 0 R` | `None` / `/Off` | 428.129, 180.135, 437.523, 189.551 |
| `CheckBox_44b` | Guarantees: Need Low | `/Btn` | 0 | 3 | `499 0 R` / `499 0 R` | `None` / `/Off` | 355.603, 159.595, 364.997, 169.011 |
| `CheckBox_47b` | Guarantees: Best new employer | `/Btn` | 0 | 3 | `500 0 R` / `500 0 R` | `None` / `/Off` | 428.129, 166.340, 437.523, 175.756 |
| `CheckBox_45b` | Guarantees: Need None | `/Btn` | 0 | 3 | `502 0 R` / `502 0 R` | `None` / `/Off` | 355.603, 145.362, 364.997, 154.778 |
| `CheckBox_48b` | Guarantees: Best IRA/new | `/Btn` | 0 | 3 | `503 0 R` / `503 0 R` | `None` / `/Off` | 428.129, 152.413, 437.523, 161.829 |
| `CheckBox_42c` | Distributions: Need High | `/Btn` | 0 | 3 | `504 0 R` / `504 0 R` | `None` / `/Off` | 355.603, 119.413, 364.997, 128.829 |
| `CheckBox_43c` | Distributions: Need Medium | `/Btn` | 0 | 3 | `508 0 R` / `508 0 R` | `None` / `/Off` | 355.603, 105.749, 364.997, 115.165 |
| `CheckBox_46c` | Distributions: Best current | `/Btn` | 0 | 3 | `509 0 R` / `509 0 R` | `None` / `/Off` | 427.866, 113.391, 437.260, 122.807 |
| `CheckBox_44c` | Distributions: Need Low | `/Btn` | 0 | 3 | `512 0 R` / `512 0 R` | `None` / `/Off` | 355.603, 91.690, 364.997, 101.106 |
| `CheckBox_47c` | Distributions: Best new employer | `/Btn` | 0 | 3 | `514 0 R` / `514 0 R` | `None` / `/Off` | 427.866, 98.596, 437.260, 108.012 |
| `CheckBox_45c` | Distributions: Need None | `/Btn` | 0 | 3 | `516 0 R` / `516 0 R` | `None` / `/Off` | 355.603, 77.435, 364.997, 86.851 |
| `CheckBox_48c` | Distributions: Best IRA/new | `/Btn` | 0 | 3 | `517 0 R` / `517 0 R` | `None` / `/Off` | 427.866, 84.669, 437.260, 94.085 |
| `CheckBox_42d` | Control: Need High | `/Btn` | 0 | 4 | `596 0 R` / `596 0 R` | `None` / `/Off` | 355.313, 678.358, 364.707, 687.774 |
| `CheckBox_43d` | Control: Need Medium | `/Btn` | 0 | 4 | `591 0 R` / `591 0 R` | `None` / `/Off` | 355.313, 664.694, 364.707, 674.110 |
| `CheckBox_44d` | Control: Need Low | `/Btn` | 0 | 4 | `592 0 R` / `592 0 R` | `None` / `/Off` | 355.313, 650.636, 364.707, 660.052 |
| `CheckBox_45d` | Control: Need None | `/Btn` | 0 | 4 | `588 0 R` / `588 0 R` | `None` / `/Off` | 355.313, 636.380, 364.707, 645.796 |
| `CheckBox_46d` | Control: Best current | `/Btn` | 0 | 4 | `590 0 R` / `590 0 R` | `None` / `/Off` | 428.277, 671.898, 437.671, 681.314 |
| `CheckBox_47d` | Control: Best new employer | `/Btn` | 0 | 4 | `587 0 R` / `587 0 R` | `None` / `/Off` | 428.277, 657.103, 437.671, 666.519 |
| `CheckBox_48d` | Control: Best IRA/new | `/Btn` | 0 | 4 | `589 0 R` / `589 0 R` | `None` / `/Off` | 428.277, 643.176, 437.671, 652.592 |
| `CheckBox_42e` | Consolidation: Need High | `/Btn` | 0 | 4 | `593 0 R` / `593 0 R` | `/Off` / `/Off` | 355.313, 621.599, 364.707, 631.015 |
| `CheckBox_43e` | Consolidation: Need Medium | `/Btn` | 0 | 4 | `595 0 R` / `595 0 R` | `None` / `/Off` | 355.313, 607.935, 364.707, 617.351 |
| `CheckBox_44e` | Consolidation: Need Low | `/Btn` | 0 | 4 | `576 0 R` / `576 0 R` | `None` / `/Off` | 355.313, 593.876, 364.707, 603.292 |
| `CheckBox_45e` | Consolidation: Need None | `/Btn` | 0 | 4 | `579 0 R` / `579 0 R` | `None` / `/Off` | 355.313, 579.621, 364.707, 589.037 |
| `CheckBox_46e` | Consolidation: Best current | `/Btn` | 0 | 4 | `594 0 R` / `594 0 R` | `None` / `/Off` | 428.277, 615.139, 437.671, 624.555 |
| `CheckBox_47e` | Consolidation: Best new employer | `/Btn` | 0 | 4 | `577 0 R` / `577 0 R` | `None` / `/Off` | 428.277, 600.344, 437.671, 609.760 |
| `CheckBox_48e` | Consolidation: Best IRA/new | `/Btn` | 0 | 4 | `584 0 R` / `584 0 R` | `None` / `/Off` | 428.277, 586.417, 437.671, 595.833 |
| `CheckBox_42f` | Creditor/legal protection: Need High | `/Btn` | 0 | 4 | `581 0 R` / `581 0 R` | `/Off` / `/Off` | 355.313, 565.497, 364.707, 574.913 |
| `CheckBox_43f` | Creditor/legal protection: Need Medium | `/Btn` | 0 | 4 | `578 0 R` / `578 0 R` | `None` / `/Off` | 355.313, 551.833, 364.707, 561.249 |
| `CheckBox_44f` | Creditor/legal protection: Need Low | `/Btn` | 0 | 4 | `583 0 R` / `583 0 R` | `None` / `/Off` | 355.313, 537.774, 364.707, 547.190 |
| `CheckBox_45f` | Creditor/legal protection: Need None | `/Btn` | 0 | 4 | `585 0 R` / `585 0 R` | `/Off` / `/Off` | 355.313, 523.519, 364.707, 532.935 |
| `CheckBox_46f` | Creditor/legal protection: Best current | `/Btn` | 0 | 4 | `580 0 R` / `580 0 R` | `None` / `/Off` | 428.277, 558.446, 437.671, 567.862 |
| `CheckBox_47f` | Creditor/legal protection: Best new employer | `/Btn` | 0 | 4 | `582 0 R` / `582 0 R` | `None` / `/Off` | 428.277, 544.241, 437.671, 553.657 |
| `CheckBox_48f` | Creditor/legal protection: Best IRA/new | `/Btn` | 0 | 4 | `586 0 R` / `586 0 R` | `None` / `/Off` | 428.277, 530.314, 437.671, 539.730 |
| `CheckBox_42g` | Sever employer relationship: Need High | `/Btn` | 0 | 4 | `562 0 R` / `562 0 R` | `None` / `/Off` | 355.313, 508.825, 364.707, 518.241 |
| `CheckBox_43g` | Sever employer relationship: Need Medium | `/Btn` | 0 | 4 | `563 0 R` / `563 0 R` | `None` / `/Off` | 355.313, 495.161, 364.707, 504.577 |
| `CheckBox_44g` | Sever employer relationship: Need Low | `/Btn` | 0 | 4 | `565 0 R` / `565 0 R` | `/Off` / `/Off` | 355.313, 481.103, 364.707, 490.519 |
| `CheckBox_45g` | Sever employer relationship: Need None | `/Btn` | 0 | 4 | `569 0 R` / `569 0 R` | `/Off` / `/Off` | 355.313, 466.847, 364.707, 476.263 |
| `CheckBox_46g` | Sever employer relationship: Best current | `/Btn` | 0 | 4 | `564 0 R` / `564 0 R` | `None` / `/Off` | 428.277, 501.774, 437.671, 511.190 |
| `CheckBox_47g` | Sever employer relationship: Best new employer | `/Btn` | 0 | 4 | `574 0 R` / `574 0 R` | `None` / `/Off` | 428.277, 487.570, 437.671, 496.986 |
| `CheckBox_48g` | Sever employer relationship: Best IRA/new | `/Btn` | 0 | 4 | `572 0 R` / `572 0 R` | `None` / `/Off` | 428.277, 473.643, 437.671, 483.059 |
| `CheckBox_42h` | Other factor: Need High | `/Btn` | 0 | 4 | `566 0 R` / `566 0 R` | `None` / `/Off` | 355.313, 452.154, 364.707, 461.570 |
| `CheckBox_43h` | Other factor: Need Medium | `/Btn` | 0 | 4 | `568 0 R` / `568 0 R` | `None` / `/Off` | 355.313, 438.490, 364.707, 447.906 |
| `CheckBox_44h` | Other factor: Need Low | `/Btn` | 0 | 4 | `571 0 R` / `571 0 R` | `None` / `/Off` | 355.313, 424.431, 364.707, 433.847 |
| `CheckBox_45h` | Other factor: Need None | `/Btn` | 0 | 4 | `560 0 R` / `560 0 R` | `None` / `/Off` | 355.313, 410.176, 364.707, 419.592 |
| `CheckBox_46h` | Other factor: Best current | `/Btn` | 0 | 4 | `567 0 R` / `567 0 R` | `None` / `/Off` | 428.277, 445.103, 437.671, 454.519 |
| `CheckBox_47h` | Other factor: Best new employer | `/Btn` | 0 | 4 | `575 0 R` / `575 0 R` | `None` / `/Off` | 428.277, 430.898, 437.671, 440.314 |
| `text_05` | Months until planned retirement | `/Tx` | 0 | 2 | `1559 0 R` / `1559 0 R` | `blank` / `None` | 177.086, 528.301, 237.528, 541.943 |
| `text_07` | Other factor description line 2 | `/Tx` | 0 | 4 | `570 0 R` / `570 0 R` | `None` / `None` | 41.951, 424.086, 342.593, 437.728 |
| `Signature1` | Retirement investor signature (downstream) | `/Sig` | 0 | 5 | `599 0 R` / `599 0 R` | `None` / `None` | 162.460, 113.427, 575.547, 138.958 |
| `text_06` | Other factor description line 1 | `/Tx` | 0 | 4 | `573 0 R` / `573 0 R` | `None` / `None` | 151.983, 437.930, 342.593, 451.572 |
| `Date_05_af_date` | Client signing date (downstream) | `/Tx` | 0 | 5 | `598 0 R` / `598 0 R` | `None` / `None` | 162.394, 61.758, 295.978, 87.085 |
| `CheckBox_48h` | Other factor: Best IRA/new | `/Btn` | 0 | 4 | `561 0 R` / `561 0 R` | `None` / `/Off` | 428.277, 416.971, 437.671, 426.387 |
| `CheckBox_49` | Remain current plan | `/Btn` | 0 | 6 | `603 0 R` / `603 0 R` | `None` / `/Off` | 463.016, 646.711, 472.410, 656.127 |
| `CheckBox_50` | Rollover to new employer plan | `/Btn` | 0 | 6 | `604 0 R` / `604 0 R` | `None` / `/Off` | 463.016, 620.664, 472.410, 630.080 |
| `CheckBox_51` | Rollover plan to IRA | `/Btn` | 0 | 6 | `605 0 R` / `605 0 R` | `None` / `/Off` | 463.016, 594.561, 472.410, 603.977 |
| `CheckBox_52` | Rollover/transfer account to new IRA | `/Btn` | 0 | 6 | `606 0 R` / `606 0 R` | `None` / `/Off` | 463.016, 568.678, 472.410, 578.094 |
| `CheckBox_53` | Rollover account to employer plan | `/Btn` | 0 | 6 | `608 0 R` / `608 0 R` | `None` / `/Off` | 463.016, 542.817, 472.410, 552.233 |
| `CheckBox_54` | Change account type | `/Btn` | 0 | 6 | `607 0 R` / `607 0 R` | `None` / `/Off` | 463.016, 516.868, 472.410, 526.284 |
| `CheckBox_55` | Fees: actual information | `/Btn` | 0 | 7 | `616 0 R` / `616 0 R` | `None` / `/Off` | 243.074, 623.335, 252.468, 632.751 |
| `CheckBox_56` | Fees: benchmarks | `/Btn` | 0 | 7 | `617 0 R` / `617 0 R` | `None` / `/Off` | 377.614, 623.335, 387.008, 632.751 |
| `text_08` | Printed investor name | `/Tx` | 0 | 5 | `597 0 R` / `597 0 R` | `None` / `None` | 162.230, 87.345, 575.757, 113.548 |
| `text_09` | Fees: Investments / Current | `/Tx` | 0 | 7 | `610 0 R` / `610 0 R` | `None` / `None` | 350.884, 506.077, 426.089, 543.185 |
| `text_10` | Fees: Investments / New employer | `/Tx` | 0 | 7 | `611 0 R` / `611 0 R` | `None` / `None` | 426.038, 506.208, 501.242, 543.316 |
| `text_11` | Fees: Investments / IRA/new | `/Tx` | 0 | 7 | `609 0 R` / `609 0 R` | `None` / `None` | 501.060, 506.077, 576.264, 543.185 |
| `text_12` | Fees: Investment services / Current | `/Tx` | 0 | 7 | `613 0 R` / `613 0 R` | `None` / `None` | 350.884, 468.631, 426.089, 505.739 |
| `text_13` | Fees: Investment services / New employer | `/Tx` | 0 | 7 | `612 0 R` / `612 0 R` | `None` / `None` | 426.038, 468.763, 501.242, 505.871 |
| `text_14` | Fees: Investment services / IRA/new | `/Tx` | 0 | 7 | `614 0 R` / `614 0 R` | `None` / `None` | 501.060, 468.631, 576.264, 505.739 |
| `text_15` | Fees: Administrative / Current | `/Tx` | 0 | 7 | `615 0 R` / `615 0 R` | `None` / `None` | 351.060, 416.865, 426.264, 468.557 |
| `text_16` | Fees: Administrative / New employer | `/Tx` | 0 | 7 | `618 0 R` / `618 0 R` | `None` / `None` | 426.038, 416.734, 501.242, 468.688 |
| `text_17` | Fees: Administrative / IRA/new | `/Tx` | 0 | 7 | `619 0 R` / `619 0 R` | `None` / `None` | 501.060, 416.865, 576.264, 468.557 |
| `text_18` | Fees: Total / Current | `/Tx` | 0 | 7 | `620 0 R` / `620 0 R` | `None` / `None` | 350.884, 393.916, 426.089, 416.528 |
| `text_19` | Fees: Total / New employer | `/Tx` | 0 | 7 | `621 0 R` / `621 0 R` | `None` / `None` | 426.038, 393.960, 501.242, 416.659 |
| `text_20` | Fees: Total / IRA/new | `/Tx` | 0 | 7 | `622 0 R` / `622 0 R` | `None` / `None` | 501.060, 393.916, 576.264, 416.528 |
| `Signature2_es_:signer1:signature` | Client consent signature (downstream) | `/Tx` | 2 | 1 | `1819 0 R` / `1819 0 R` | `None` / `None` | 71.227, 285.348, 181.742, 307.348 |

## Date field actions

- Rollover `Date_04_af_date_es_:prefill`: format and keystroke JavaScript actions for `mm/dd/yyyy`.
- Intake `Date_05_af_date`: same actions, but belongs to downstream client signing.
- Rollover `Date_es_:signer:date`: text field with required flag; no format/keystroke action discovered.

## Duplicate and default audit

- No duplicated fully qualified names within either PDF.
- No multi-widget fields and no unmatched page widgets.
- Both consent pages reuse the same signature name across documents; namespace every field by document instance.
- All `/DV` defaults absent. New-case initialization must not treat template `/V` selections as established facts.
- Actual signature field `Signature1` has no signed value. Signature-like text fields have no signing evidence.
