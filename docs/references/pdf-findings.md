---
node_id: ref-pdf-findings
type: reference
title: Technical findings from the two supplied TWS PDFs
created: 2026-09-14
updated: 2026-09-14
status: active
category: reference
tags: [pdf, evidence]
summary: Read-only structural and visual inspection, exact field counts, anomalies and mapping implications.
---

# B. Findings from the two real PDFs

## Inspection method and limits

Inspected the two original files in the project root on 2026-09-14 using pypdf field-tree/page-widget enumeration, extracted page text, pdf-lib **1.17.1** read-only loading/type enumeration, and Poppler rendering of **all ten pages** at a 1400-pixel long edge. Followed `/Parent` and `/Kids`, compared field names/objects to widgets, inspected `/V`, `/AS`, `/AP/N`, flags, date actions, and signature placement. No originals were written, normalized, filled, or flattened. Analysis scripts/renders reside outside the project; only this evidence and inventory are delivered here.

**Proven:** both are loadable AcroForms with the field types below. **Not yet proven:** editing/output roundtrip, appearance regeneration, live PDF.js integration, or compatibility with a downstream signing service. Those are M0 acceptance gates, not completed app tests.

## Summary

| Property | Rollover | Intake / Recommendation |
|---|---:|---:|
| File | `TWS_Rollover Attestation_(FILLABLE)_10.31.2022.pdf` | `TWS_Intake_Recommendation_Attestation_Form_(FILLABLE)_10.31.2022.pdf` |
| Bytes | 580,699 | 699,748 |
| PDF version | 1.6 | 1.6 |
| Pages | 3 | 7 |
| Page geometry | All 612 x 792 points, rotation 0 | All 612 x 792 points, rotation 0 |
| AcroForm fields / widgets | 14 / 14 | 175 / 175 |
| Text fields (`/Tx`) | 10 | 22 |
| Checkboxes (`/Btn`, flags 0) | 4 | 152 |
| Actual signature fields (`/Sig`) | 0 | 1 |
| Actual radio groups / choice fields | 0 / 0 | 0 / 0 |
| Duplicate field names / multiple widgets per field | 0 / 0 | 0 / 0 |
| Widgets unmatched to canonical fields | 0 | 0 |
| XFA / encryption | Absent / no | Absent / no |
| `/NeedAppearances` | Absent | Absent |
| `/SigFlags` | 0 | 1 (does not establish a signature exists) |

SHA-256 originals:

- Rollover: `1cf50ec1d712c6063b7cc212689937dedfb724573312ef7a1719a68c4ad03534`
- Intake: `2033f25c00beee6c5c152507b3a0ce45df826c0507b3b7cf1e81afee3e0cb6e3`

## Rollover - three pages

| Page | Widgets | Content and mapping significance |
|---|---:|---|
| 1 | 1 | Electronic records/signature consent; client consent signature stored as a text field |
| 2 | 9 | Individual name, sponsor/employer, current plan, sponsor contact; three employment choices; departure date and Other description |
| 3 | 4 | Conditional recommendation disclosure checkbox; retirement-investor signature, printed name and signing date |

Seven text fields are non-signing preparation inputs: five ordinary fields on page 2, a departure-date field on page 2, and printed name on page 3. Three remaining text fields are two client signature placeholders and the client signing date. A field name beginning with `Signature` does not mean its PDF field type is `/Sig`.

Employment choices explicitly say "PLEASE CHECK ONE". Configure `CheckBox_01_es_:prefill`, `CheckBox_02_es_:prefill`, and `CheckBox_03_es_:prefill` as one enum. Selecting departure requires the date; selecting Other requires description. Do not map the first choice merely from `currentlyEmployed`: its printed statement also asserts eligibility for an in-service distribution. Use an explicit case-specific attestation choice.

`CheckBox_04_es_:prefill` is a separate substantive disclosure, not a fourth employment choice. Never infer its truth from a recommendation or check it by default. It is logically `/1` in the supplied file and must be explicitly reset in each new case's working state.

**Demo value:** quickly shows shared plan facts, direct overrides, conditional validation, and signature-role separation.

## Intake - seven pages

| Page | Widgets | Content and mapping significance |
|---|---:|---|
| 1 | 1 | Client electronic-consent signature placeholder (`/Tx`, required PDF flag) |
| 2 | 36 | Name/plan/account information, employment branches, three Provided/Not Provided pairs, three High/Medium/Low alignment columns |
| 3 | 78 | Five service rows, each with four-level need plus three Yes/No comparisons; four factor rows, each with need plus best-alignment choices |
| 4 | 37 | Five further factor rows (including Other), two description lines, then printed disclosure text |
| 5 | 3 | Retirement-investor attestation: actual `/Sig`, printed name, signing date |
| 6 | 6 | Six recommendation choices; no automatic recommendation calculation |
| 7 | 14 | "Internal Use Only" fee comparison: Actual/Benchmarks pair plus 12 text cells (four rows x three account columns) |

Of its 22 text fields: 20 are `text_01` through `text_20`; one is the signing date and one a consent signature placeholder. `/Sig Signature1` is empty and belongs to the retirement investor. No actual advisor signature field was found.

Checkbox groups are implemented as **independent AcroForm checkbox fields**, not radio-button parents. The suffixes `a` through `h` repeat a naming pattern for different matrix rows; they are not duplicate widgets. Numeric field order is not reading order. Locate fields by exact name plus page/rectangle evidence.

### Semantic choice map

| Fields | Intended interpretation |
|---|---|
| `CheckBox_01`-`07` | Account type: 401(k), 403(b) ERISA, 403(b) non-ERISA, 457, pension/DB, other employer plan, IRA |
| `08`, `09` | N/A toggles for sponsor and current plan; paired text fields must not conflict |
| `10`, `13` | Currently employed / no longer employed by plan sponsor |
| `11`, `12` | Planning retirement within months / not planning departure, applicable within current employment |
| `14`, `15`, `16` | Retired / employed by new employer / not currently employed; semantics can overlap in real life, so confirm firm policy rather than assuming all three exclusive |
| `17/18`, `19/20`, `21/22` | Provided / Not Provided for quarterly statements, fee disclosure, plan description |
| `23/24/25`, `26/27/28`, `29/30/31` | One High/Medium/Low choice per account column |
| `32/33/34/35` + suffix `a`-`d` | Need High/Medium/Low/None for monitoring, allocation, advice, management, planning |
| `36/37`, `38/39`, `40/41` + same suffixes | Yes/No per current/new-employer/IRA column, independently for each service row |
| `42/43/44/45` + suffix `a`-`h` | Need High/Medium/Low/None for tax, beneficiary, guarantees, distributions, control, consolidation, protection, severing employer relationship, Other |
| `46/47/48` + same suffixes | Best alignment: current plan / new employer plan / IRA; proposed single selection per row, subject to firm review |
| `49`-`54` | Recommendation list; Stage 1 single-transaction assumption: one selection, declared in config |
| `55/56` | Fee comparison basis: actual plan information / benchmarks |

**Demo value:** proves the architecture supports substantive case questions and matrices instead of treating every field as client demographics.

## Unusual AcroForm details and controls

1. **Export values:** all 156 checkboxes use PDF name `/1` as on value. The off value is `/Off`; the normal appearance dictionaries contain only `/1`, without an explicit `/Off` appearance. pdf-lib reads these as `PDFCheckBox`. Do not use string `true` or assume `/Yes`.
2. **Unexpected stored selections:** intake `CheckBox_07` (IRA) and `CheckBox_17` (quarterly statements Provided), plus rollover `CheckBox_04_es_:prefill`, have both `/V /1` and `/AS /1`. These are not advisor decisions. Intake `text_01` contains a single space. All new-case factual defaults must be explicitly cleared/configured.
3. **Appearance concern:** the logically selected boxes did not show a discernible check at the inspected render scale. On-state streams exist and use a `/ZaDb` glyph `(4)`. Do not diagnose the precise font/render cause without the spike; require a readable checked mark after regeneration and test unchecked appearance as well.
4. **Date actions:** rollover `Date_04_af_date_es_:prefill` and intake `Date_05_af_date` have `/AA` JavaScript format/keystroke actions invoking `AFDate_FormatEx("mm/dd/yyyy")` and `AFDate_KeystrokeEx("mm/dd/yyyy")`. Date is a text subtype; implement date formatting/validation in application config, never execute embedded PDF JavaScript. No root `/OpenAction` or `/Names` entry was found.
5. **Adobe-style names:** rollover has an empty `/ADBE_EchoSign` dictionary and signature/prefill tagging in field names. Intake has a tagged consent placeholder too. These are not an implemented signing integration. Preserve the originals and exact names; downstream compatibility remains unproven.
6. **Required flags:** rollover's two signature placeholders and signing date carry `/Ff 2`; intake's consent placeholder carries `/Ff 2`. Other inventoried fields have `/Ff 0`; every widget has `/F 4` (print). PDF-required flags are not sufficient preparation rules and must not block the advisor on downstream client signing.
7. **No duplicate widgets found:** canonical fields and page annotations matched, no repeated names within either document, no field had multiple widgets. Adjacent printed-name/signature rectangles are separate fields. Their shared names across two different PDFs must be namespaced by document instance.
8. **Internal material:** intake page 7 must have an explicit export audience. Client exports are derived copies with a tested page/field policy; internal archive keeps all seven pages. Never delete the original page.

## Initial conceptual mapping examples

| Meaning | Rollover field | Intake field | Owner |
|---|---|---|---|
| Person full name | `text_01_es_:prefill` | `text_01` | Derived from active-case client snapshot |
| Plan sponsor | `text_02_es_:prefill` | `text_03` | Case current plan, not automatically current employer |
| Current plan name | `text_03_es_:prefill` | `text_04` | Case |
| Sponsor contact | `text_04_es_:prefill` | No matching field | Case |
| Printed investor name | `text_06_es_:prefill` | `text_08` | Client-derived; can be overridden independently |
| Employment attestation | First three checkboxes | Different employment hierarchy | Distinct case semantics; no blind shared mapping |
| Statements availability | None | `CheckBox_17/18` | Case requested documentation enum |
| Fee comparison | None | `text_09`-`text_20` | Case fee cells; units must be explicit |
| Client signing | Two tagged text placeholders + date | Consent text placeholder + `/Sig` + date | Downstream client workflow; excluded from app signing |
| Address / SSN / DOB / beneficiaries | None | None | Use only relevant sample fields in Stage 1; no invented TWS mappings |

## Feasibility conclusion

**pdf-lib is a realistic candidate**, supported by successful read-only enumeration of every expected field and absence of XFA/encryption. It is not a viewer or an interactive PDF editor. [Its form API](https://pdf-lib.js.org/docs/api/classes/pdfform) supplies field access and appearance/flattening operations; [PDF.js](https://mozilla.github.io/pdf.js/api/) supplies display capabilities. Their joint live-sync and export behavior must be proven on copies before claiming compatibility. Full flattening is not the default because downstream client signature fields must survive.

Complete discovered names/types/values/locations: [AcroForm appendix](pdf-field-inventory.md).
