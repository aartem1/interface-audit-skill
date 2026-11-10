# Interface audit report v1

The report is the handoff between audit and repair. Use UTF-8 JSON and the adjacent [JSON Schema](report.schema.json). The audit snapshot describes what was observed on the audited revision. Repair history describes what happened on later revisions.

## Identity and scope

| Field | Contract |
| --- | --- |
| `format_version` | Integer `1`. Reject unsupported versions before writing. |
| `audit_id` | Unique within the report store. Use a UTC timestamp and short target slug, plus a suffix if needed. |
| `created_at` | UTC ISO 8601 timestamp. Use this field for sorting. |
| `target` | A human-readable `label`, stable `identity` used to match the project or artifact, `kind`, observed `location`, nullable `revision`, and `environment`. Use a sanitized origin, repository plus app path, app ID, or artifact ID as identity. |
| `scope` | `mode` is `targeted` or `whole_accessible`. Record `included`, `excluded`, `assumptions`, and `limits` as arrays of strings. |
| `cases` | Enumerated items in the test plan. Use one ID per target and test context, such as a checkout failure on a named device and role. Use `exercised`, `partial`, `not_exercised`, or `excluded` as coverage. Explain any status other than `exercised`. |
| `checks` | One record per criterion, case, and subject. Assign each a stable ID such as `C-001`. Include the source for every `LOCAL-*` check in `criterion_source`. |
| `findings` | Confirmed defects linked to one or more failed check IDs. Each finding has a stable ID such as `F-001`. Finding IDs are scoped to the audit ID. |

Use stable case IDs such as `CASE-001`. Add one check row per planned combination of case, criterion and subject, including planned checks that cannot run. Coverage is `exercised` when the planned examination of the case is complete, `partial` when some of it ran, `not_exercised` when none ran, and `excluded` when it was inventoried and deliberately left outside scope. Do not add checks for cases marked `excluded`. Record inaccessible roles or undiscoverable surfaces in `scope.limits` even if you cannot enumerate their cases.

## Check outcomes

| Status | Verdict | Required record |
| --- | --- | --- |
| `checked` | `true` | Steps, expected and observed result, and at least one evidence reference. |
| `checked` | `false` | The same fields, with a reproducible counterexample. |
| `not_applicable` | `null` | Reason why the criterion's applicability condition does not hold. |
| `not_checked` | `null` | Missing observation, tool, data, or access. |

Aggregate by criterion over the declared scope. Any confirmed failure gives a failed criterion. Otherwise any planned check that remains unverified gives an incomplete criterion. Otherwise at least one pass gives a passed criterion. All inapplicable gives an inapplicable criterion. Show coverage gaps even when a criterion has already failed. Untested and out-of-scope work are not confirmed defects.

## Findings and repair history

Record a finding's `title`, `severity`, `impact`, failed `check_ids`, and measurable `acceptance` conditions. The linked checks hold the reproduction context. Pick severity by observed user impact in scope.

| Severity | Impact |
| --- | --- |
| `critical` | An in-scope critical task is blocked without a usable workaround, or the failure can trigger an irreversible or materially harmful outcome. |
| `high` | An in-scope primary task or supported access path is blocked, or completing it requires leaving the normal workflow for a workaround. |
| `medium` | Users can finish in the normal workflow but face extra work, a recoverable error, or missing information. |
| `low` | A visible inconsistency or minor defect with no demonstrated effect on task completion. |

Each finding starts with `fix.status: "open"` and empty `fix.history`. Repair attempts append to history. When history is nonempty, `fix.status` must match its latest attempt. Each attempt records `at`, `status`, `revision`, `environment`, `changes`, `verification`, and `note`. A verification entry records `check`, `result` (`pass`, `fail`, or `not_run`), and `evidence`. Link evidence for each observed pass or failure. Use a note for the exact unresolved condition on `blocked` or `implemented_unverified`.

| Repair status | Meaning |
| --- | --- |
| `open` | Repair is pending, or current evidence shows the condition still fails. |
| `blocked` | Access, a dependency, or another named condition prevents repair or establishing a baseline. |
| `implemented_unverified` | A change is present and at least one required check is still pending, with no observed failure in the checks that ran. |
| `fixed` | The repair is present or attributable to an identified change, all acceptance checks passed, and relevant regressions were checked. |
| `not_reproduced` | The audited failure was not reproducible after equivalent acceptance checks passed on the current target without a change in this attempt, and no verified repair can be attributed. |

Preserve IDs, severity, original check results, and prior attempts. Only `fix` fields change during repair. Every case, check, and finding ID must be unique in its list. Every check must link to a case. Findings must cover all failed checks, and every finding must link only to failed checks. Case, check, and finding links are semantic validations in addition to schema validation.

## Write and resume

Create the audit report at a new path. For a repair checkpoint, re-read the current file and merge the new attempt into the same finding. Preserve changes to other findings. Serialize writes to one report. If independent writers cannot coordinate, keep separate updates. If the selected finding has changed since you read it, or you cannot safely update the report, preserve your result as a separate update instead of replacing someone else's work. Prefer atomic replacement where the editing surface supports it. A structured update contains `format_version`, `audit_id`, and `updates`, each with `finding_id` and the new `attempt` in report format.

Evidence can be a relative path, a stable artifact reference, or a short textual observation with its measurement conditions. Use evidence from the correct version and environment. Redact secrets and identifying data before saving any report or attachment.
