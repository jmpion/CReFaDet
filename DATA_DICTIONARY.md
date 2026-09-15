# CReFaDet data dictionary

This document describes CReFaDet release 1.0.0. The authoritative data file is `CReFaDet-release-v1.xlsx`.

## Workbook structure

### `Customer Review Data`

This sheet contains 2,415 records. `review_id` is unique and should be used as the record key.

| Column | Type | Description |
| --- | --- | --- |
| unnamed first column | integer | Zero-based row index from 0 to 2,414; not a semantic identifier |
| `review_id` | string | Unique review identifier |
| `tablet_id` | categorical string | Tablet-model identifier; 11 models are represented |
| `comment` | text | Customer-review text |
| `stars` | integer | Customer rating from 1 to 5 |
| `failure_class` | categorical string or blank | `IF` = intolerable failure; `TF` = tolerable failure; blank = no failure label |
| `review_date` | date or blank | Review date in `YYYY-MM-DD` display format; populated for 1,200 reviews from the 10 non-Asus-C302 models |
| `usage_time` | duration label, `X`, or blank | Failure-free usage duration; annotated for 859 non-failure reviews of the Asus C302 model |

`usage_time` contains 71 explicit duration labels and 788 `X` values. Here, `X` means that no explicit failure-free duration was stated. A blank means that this annotation was outside the row's scope.

### `Component labels`

This sheet contains 356 failure reviews for the Asus C302 model. Every `review_id` links to exactly one row in `Customer Review Data`.

The sheet has these column groups:

| Columns | Type | Description |
| --- | --- | --- |
| `review_id` | string | Foreign key to `Customer Review Data.review_id` |
| 60 component columns, from `Hinge` through `Volume control` | integer category | Component-level failure annotation: `0` = no failure mention, `1` = failure, `2` = ambiguous or uncertain evidence |
| `failure_comment_summary` | text | Concise summary of the failure information in the review |
| `uncertain_data_flag` | integer or blank | `1` marks a record with uncertain annotation; blank otherwise |
| `tid` | duration label or blank | Time of initial degradation, when explicitly stated |
| `tff` | duration label or blank | Time of final failure, when explicitly stated |

`tid` and `tff` are both present for 138 records and both blank for the remaining 218. When a review does not distinguish initial degradation from final failure, the annotation policy sets `tid = tff`. `X` is not stored in these two columns.

### Public workbook scope

The public workbook contains `Customer Review Data` and `Component labels`.

## Failure classes

| Value | Count | Meaning |
| --- | ---: | --- |
| `IF` | 417 | Intolerable failure |
| `TF` | 405 | Tolerable failure |
| blank | 1,593 | No failure label |

For binary failure detection, combine `IF` and `TF` as the positive class. For severity classification, restrict the analysis to failure records and distinguish `IF` from `TF`.

## Duration-label grammar

Duration labels concatenate one or more `<number><unit>` terms without spaces. Supported units are case-sensitive:

| Unit | Meaning | Normalized days |
| --- | --- | ---: |
| `Y` | year | 365.25 |
| `M` | month | 30.4375 |
| `W` | week | 7 |
| `D` | day | 1 |
| `H` | hour | 1/24 |
| `m` | minute | 1/1440 |

Examples include `1Y`, `12M`, `1M5D`, `3W2D`, `4H`, `30m`, and `0D`. Preserve the original label in the dataset and compute normalized days in a separate derived field. Composite durations are additive.

`0D` represents a failure or defect present on receipt, out of the box, during setup, first use, or the first day. It is an immediate/built-in-failure category, not a measured positive lifetime.

## Missingness and scope

Blank values have field-specific meanings and must not automatically be interpreted as negative labels:

- blank `failure_class`: no failure label;
- blank `review_date`: date annotation was outside the released scope;
- blank `usage_time`: usage-time annotation was outside the released scope;
- `usage_time = X`: the row was in scope, but no explicit duration was stated;
- blank `tid` and `tff`: no explicit duration was stored for that component-annotation row.

## Release validation summary

Release 1.0.0 contains 2,415 unique review identifiers with no duplicate IDs. All component-annotation IDs resolve to the main sheet. Failure classes, star ratings, dates, duration labels, and ternary component values were checked against their documented domains. No formula errors or external workbook links were found.
