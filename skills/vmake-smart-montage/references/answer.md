# Montage generation answers and edits

Both `talking_mixed_cut` and `material_mixed_cut` generate in the original session with `--brief-id`. `--answer` is optional; omitting it uses the current Brief. Do not add empty fields or require talking-head video for a material plan.

The following edits apply only to the talking-head branch. For a material plan, omit `--answer`. Changes to theme, copy, media selection, duration, script, or voice require reanalyzing the complete inputs with updated requirements in the same session, presenting the new Brief, and obtaining confirmation before generation.

| Field | Value | Constraint |
| --- | --- | --- |
| `a_roll_media` | Array of media objects | At least one actual video. |
| `b_roll_media` | Array of media objects | Images or videos; an empty array explicitly clears supporting footage. |
| `target_duration` | JSON number | A generation target of 5-180 seconds. |

Media objects contain only `short_id` and `kind`, selecting inputs already analyzed in the current plan. Take real IDs and types from the analysis or same-session history. Do not include URLs, file paths, or guessed IDs. Summary fields `a_roll_short_ids` and `b_roll_short_ids` are not answer fields. For new or replacement inputs, download remote originals according to CLI setup, then reanalyze the complete local list and confirm the new Brief. Do not preregister inputs or insert new URLs into an answer.

This example shows structure only. Replace the ID and kind with real current-plan media, and omit fields the user has not requested to change:

```json
{
  "b_roll_media": [{ "short_id": "actual-supporting-image-id", "kind": "image" }],
  "target_duration": 45
}
```

For flags, serialize the complete object as one correctly quoted `--answer` argument. For a parameter file, put the session, `briefId`, and `answer` object in the same request JSON. Do not serialize `answer` as a string or add separate business flags. Append `--validate-only --json` only for requested input checks or diagnosis. Neither form permits editing read-only speech, switching branches, arbitrary backend fields, or a material plan's script and voice.
