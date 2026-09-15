# Zapier Cutover Instructions — Tomorrow Lab Quiz Form

## Context

The live landing page currently submits through GHL form `ba8gdamks1hZIKx250ch`, which feeds a Zap that writes each submission into the client's Google Sheet. That form is **still live and driving the active ad campaign** — do not edit or touch it.

A clone of that form was created for the new 2-step quiz page (`index-v2.html`):

- **New form ID:** `AV8DG8XyEwP9RnhcTk9N`
- **New hidden fields** (added to the clone only, fed by URL params from the quiz):
  | Field | Query Key |
  |---|---|
  | Company Type (Quiz) | `company_type_quiz` |
  | Requested Services (Quiz) | `requested_services_quiz` |
  | Estimated Budget (Quiz) | `estimated_budget_quiz` |

These are brand-new fields (not conversions of the original dropdowns), so the Zap needs both a trigger change and a field re-mapping — not just a trigger swap.

## Before touching the Zap: test the clone in isolation

1. Open `index-v2.html` locally or on a staging URL and click through all 3 quiz questions.
2. On Step 2, inspect the iframe and confirm its `src` includes `company_type_quiz`, `requested_services_quiz`, and `estimated_budget_quiz` with the values you selected.
3. Submit a real test entry through the clone form.
4. In GHL, open the clone form's submissions (or the resulting contact record) and confirm all 3 hidden fields captured the correct values.

Do this against the clone — it has no effect on the live form, live page, or the Google Sheet, since the Zap isn't pointed at it yet.

## Updating the Zap (do this only at cutover time, alongside publishing the new page)

1. Open the existing Zap (trigger: "New Form Submission" scoped to a specific GHL form).
2. **Trigger step:** re-select the form dropdown from the old form to the new one (`AV8DG8XyEwP9RnhcTk9N` / whatever name the clone shows as, e.g. "New PPC Form - Copy"). Re-test the trigger step so Zapier pulls a fresh sample submission (needed so it can see the 3 new hidden fields).
3. **Action step (Google Sheets mapping):** the standard fields (name, email, phone, etc.) should still map fine since those weren't touched. But the 3 columns that previously pulled from the old dropdown fields will now show as unmapped/blank, because the new hidden fields are different underlying fields. Re-map each of those 3 Google Sheet columns to the corresponding new field from the trigger's sample data:
   - Company Type column → `Company Type (Quiz)`
   - Requested Services column → `Requested Services (Quiz)`
   - Estimated Budget column → `Estimated Budget (Quiz)`
4. Send a test through the action step (Zapier's "Test action" / "Send test to Google Sheets") to confirm the row lands with all 3 values in the right columns.
5. Turn the Zap back on if it was paused for editing.

## Cutover sequence

Do these together, no gap:

1. Publish/upload the new `index-v2.html` (quiz version) to replace the live page.
2. Immediately after (or right before), flip the Zap trigger + re-mapped columns as above.

The old form (`ba8gdamks1hZIKx250ch`) and its Zap mapping stay untouched and unused after this point — safe to leave as-is or archive later.

## Rollback

If something breaks after cutover: swap the Zap trigger back to `ba8gdamks1hZIKx250ch` and revert the Google Sheet field mappings to their original columns, and re-publish the old page. Since the old form/mapping was never modified, this is a clean revert.
