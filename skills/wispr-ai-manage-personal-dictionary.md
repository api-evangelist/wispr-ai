---
name: Manage a Wispr Flow personal dictionary
description: Read and update the personal dictionary that tunes Wispr Flow transcription
  for a user's names, jargon, and technical terms, using the Wispr backend API.
api: openapi/wispr-ai-backend-openapi-original.json
operations:
- get_personal_dictionary
- update_personal_dictionary
- delete_word_in_dictionary
generated: '2026-07-21'
method: generated
---

# Manage a Wispr Flow personal dictionary

Wispr Flow's personal dictionary teaches transcription the user's names, jargon,
and technical terms. The backend API at `https://api.wisprflow.ai` exposes it under
`/api/v1/dictionary/personal`.

## Auth

Every dictionary operation requires the `ApiKeyHeaderPatched` scheme — an API key
sent in the `Authorization` header (see `authentication/wispr-ai-authentication.yml`).
This is the app's own backend API: keys are issued to Wispr Flow clients, not via a
public developer program.

## Steps

1. **Read the current dictionary** — `get_personal_dictionary`
   (`GET /api/v1/dictionary/personal`). Returns the user's dictionary items.
2. **Update it** — `update_personal_dictionary` (`POST /api/v1/dictionary/personal`)
   with a JSON array of `DictionaryItem` objects. Send the full desired list — this
   is a replace-style write, so include existing items you want to keep.
3. **Remove a single word** — `delete_word_in_dictionary`
   (`DELETE /api/v1/user/dictionary/{word_id}`) when you only need to drop one entry.

## Error handling

- `422` returns a FastAPI `HTTPValidationError` envelope (`detail[]` of
  `loc`/`msg`/`type`) — fix the request body shape and retry.
- There is no `Idempotency-Key` contract; deletes are naturally idempotent
  (repeat deletes are safe), but treat POSTs as non-idempotent.
- See `errors/wispr-ai-problem-types.yml` and `conventions/wispr-ai-conventions.yml`.
