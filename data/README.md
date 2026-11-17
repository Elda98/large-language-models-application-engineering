# Dataset card — synthetic-v1

This dataset is fictional and authored for the course. It is not scraped, government data, or personal information. It is embedded in the core notebook and exported by Day 4; no dataset download is needed.

## Public service directory

| Key | Value |
|---|---|
| `ar` | يمكنك متابعة حالة طلبك باستخدام رقم المرجع. |
| `en` | You can check your application status using its reference number. |

The mock supports exact directory questions plus a limited clarification example. It does not understand arbitrary language.

## Application records

| Synthetic reference | Synthetic owner | Synthetic status |
|---|---|---|
| REF-1001 | demo-user-1 | under_review |
| REF-1002 | demo-user-2 | approved |

`REF-9999` intentionally does not exist. The trusted session fixture is `demo-user-1`. A reference number identifies a record but does not prove ownership. The status tool checks owner equality before returning status. User input cannot set the trusted session.

`not_found` and `access_denied` are internal test codes; the outward answer is identical and denied answers do not disclose the other status. This is an educational fixture, not complete production security.

## Ticket schema

Exactly four fields:
- `service`: `service_info`, `application_status`, or `other`.
- `city`: `Riyadh`, `Jeddah`, or `unknown`.
- `request_id`: null or `REF-` followed by four ASCII digits.
- `needs_clarification`: Boolean.

A missing reference for `application_status` must request clarification. Only Riyadh extraction is implemented; Jeddah being an allowed schema value does not mean every Jeddah place-name is parsed.

## Golden cases and privacy

C01–C10 cover Arabic FAQ, English FAQ, owned record, absent record, missing reference, out-of-scope service, another owner's record, instruction override, blank input and malformed ticket.

Ten passing fixtures show only that these listed paths work. Do not add real names, IDs, phone numbers, applications or ministry records.
