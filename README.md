# Lead Qualification — n8n & Supabase

A learning and portfolio project that captures quote requests, assigns a priority using a simple business rule, and stores the results in Supabase.

## Business rule

| Requested service | Priority |
| --- | --- |
| Exactly `Online store` | `High` |
| Any other service | `Normal` |

This is a fictional exercise rule, not a general measure of lead quality. The comparison is case-sensitive and does not trim spaces. This workflow does not use AI.

## Workflow

1. **Receive contact form** collects required `name` and `service` text fields.
2. **Build confirmation message** creates a personalized message and preserves the input fields.
3. **Is online store?** checks the service.
4. **Set high priority** or **Set normal priority** adds the appropriate value while preserving the other fields.
5. **Save contact to Supabase** inserts `name`, `service`, `message`, and `priority` into `contacts`.

The two mutually exclusive branches connect to the same save node. The message is stored in the database; no email or notification is sent. The form displays the default submission confirmation.

## Files

- `lead-qualification.json`: sanitized workflow template for import.
- `schema.sql`: fresh table setup and additive priority column for the earlier Contact Capture study table.

## Setup

1. Create a Supabase study project or use your existing study project.
2. Review and run `schema.sql` in its SQL Editor. Keep RLS enabled. For an existing table with a differently named identity sequence, adapt the sequence grant only if needed.
3. Import `lead-qualification.json` into a compatible n8n instance.
4. Open **Save contact to Supabase**, connect your own Supabase OAuth account, select your project in place of `YOUR_SUPABASE_PROJECT_REF`, and choose `contacts`.
5. Verify the four field mappings use their corresponding `$json` values.
6. Execute the workflow and submit the test form for both branches.
7. Publish the configured workflow, then open its own Production URL and test without manually executing it.

The template imports inactive and has a new form webhook identifier. It does not reuse the author's live form URL. Node versions are preserved from the tested export: Form Trigger 2.6, Edit Fields 3.5, If 2.3, and Supabase 1. OAuth availability may depend on your n8n environment.

## Validation performed on the original workflow

| Input | Expected | Observed |
| --- | --- | --- |
| Lucas / Online store, test form | High | Saved successfully |
| Mariana / Landing page, test form after correction | Normal | Saved successfully |
| Pedro / Online store, production form | High | Saved in Supabase without manual execution |

The exported configuration was reviewed and the sanitized JSON was structurally checked. The sanitized template itself has not been reimported and executed.

## Limitations and next steps

- Required fields do not validate whether a name or service is meaningful.
- No email or phone is collected, so this demo does not provide a way to contact the lead.
- Repeated submissions create new rows; no duplicate detection is implemented.
- No spam protection, custom error workflow, or external notifications are configured.
- Input normalization and a service dropdown would improve classification consistency.
- Availability of a live demo depends on the hosting account and plan.

## Security

Credential references, instance metadata, and the original project reference were removed from the public template. Configure credentials inside n8n; never commit API keys or database passwords. Do not disable RLS or grant anonymous access for this workflow. Use fictional data in public screenshots.
