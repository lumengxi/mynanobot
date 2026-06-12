---
name: feed-draft-writer
description: Polish or rewrite a 1001APPS Homebase feed-card draft (customer replies, invoice reminders, review responses, win-back emails) in the owner's voice, grounded only in the card's evidence.
metadata: {"nanobot":{"emoji":"📝"}}
---

# Feed Draft Writer

Homebase (the 1001APPS UI) generates feed cards whose drafts are deterministic templates.
This skill turns one of those drafts into a sharper, more human version — without
inventing a single fact.

## When to use (trigger phrases)

- "polish this draft" / "rewrite this reminder" / "make this reply warmer"
- Any prompt that pastes a 1001APPS feed card (title + facts + draft) and asks to improve it
- Prompts that reference a card by name, e.g. "rewrite the Oakmont HOA reminder"

## Input you can expect

The user (or Homebase via the composer) pastes some of:

- The card title, e.g. `Oakmont HOA owes $7,714 — 8 days late`
- Fact chips, e.g. `Amount $7,714 · Days late 8 · Tone Firm`
- Evidence excerpts (email snippets, invoice lines, review text)
- The current draft body

If you need the underlying source records, they live in the Homebase workspace at
`workbench/homebase/data/faker/<org-slug>/` as JSONL (`emails.jsonl`,
`quickbooks_invoices.jsonl`, `reviews.jsonl`, ...). Use `grep` to find a specific
record (e.g. an invoice docNumber) instead of loading whole files.

## Voice rules

1. Owner-operator voice: first person, plain words, short sentences. No corporate filler
   ("per my last email", "kindly", "at your earliest convenience").
2. Keep every number, date, name, and invoice/document id EXACTLY as given. Never add
   amounts, dates, or promises that are not in the card's facts or evidence.
3. Match the card's stated tone (`Friendly` / `Firm` / `Final notice` for reminders;
   apologetic-but-owned for ≤3-star reviews; grateful and brief for 4–5 stars).
4. Default length: under 120 words. One ask per message. Sign with the owner's first
   name and business name when present in the draft being replaced.
5. Public review replies: never argue, never share customer details, invite the
   conversation offline for anything sensitive.

## Output format

Return ONLY the replacement draft body (plus a `Subject:` line if the original had one).
No commentary, no markdown fences — the user pastes your output straight into the
card's Edit view in Homebase.
