# AURA Handoff

Paste this at the start of a new conversation. Date: October 27, 2026. Supersedes the October 15 handoff.

## Who and what

I'm building AURA, the flagship accessible AI orchestrator of Stand Alone Braille Solutions, for blind and low-vision professionals who use screen readers and Braille displays. Repo zmason005/AURA is now private, so the GitHub Pages skeleton is down, which is expected. ANLLMS and the Knowledge Base are connected products. AURA's own backend will be on Cloudflare (Pages plus Functions).

## How I want to work

- Explain the reasoning first. Then give one clear action at a time, and ask me a single yes/no on the step you judge most logical.
- Setup steps: many small steps, one action each, with placemarkers saying what I should hear. I use iPhone with VoiceOver, so use element names, not positions or colors. Say when wording may vary. Warn before any tap that runs something or can't be undone.
- Always write the full name LITELLM_MASTER_KEY, never "master key".
- Secrets: never ask me to paste or read values into chat. I use headphones for key handling. I report names, types, and "empty or filled in", never values.
- I'm front-end experienced, backend novice: plain language and analogies. Don't generate full code files without asking. Keep responses token-efficient.
- In Shortcuts: request bodies must be JSON fields (Add new field), not a Text box. Show Result isn't available, so use Show Alert. On iOS 26, add actions at the end of the list, because moving them is unreliable. My keyboard curls quotes, so I never type code.
- The Supabase connector in Claude can read function code and deploy. I approve each deploy first. Neither Claude nor the connector gets secret values.

## Where we are: hunt for old LITELLM_MASTER_KEY copies

### Done

- Vercel, Knowledge Base app: no variables.
- Supabase project knowledge-hub holds three Edge Functions. Only **ingest-conversation** calls the proxy, for embeddings (model gemini-embedding-001, dimensions 768, one request per exchange). mcp-server and upload-to-r2 don't.
- Made limited key alias `knowledge-base-proxy-key` (gemini-embedding-001 only, 60 rpm). Stored in Infisical (Knowledge-Base, Development, folder `/knowledge-base`, secret LITELLM_API_KEY).
- Infisical-to-Supabase sync is live. Connection `supabase-connection` uses a scoped token, `infisical-sync-supabase`: knowledge-hub only, Edge Function Secrets read-write plus Projects read, expires about January 25, 2027. Sync `knowledge-base-supabase-sync` goes from `/knowledge-base` to knowledge-hub, with Disable Secret Deletion on, include-subfolders on, and auto-sync on. LITELLM_API_KEY now appears in the Supabase secrets.
- Redeployed ingest-conversation as version 11, reading LITELLM_API_KEY. verify_jwt stays off. Version 10 read the old name, so it's the fallback.

### Supabase secrets now

R2_ACCOUNT_ID, R2_ACCESS_KEY_ID, R2_BUCKET_NAME, R2_SECRET_ACCESS_KEY, LITELLM_BASE_URL, LITELLM_MASTER_KEY (old, still present), MCP_ACCESS_TOKEN, LITELLM_API_KEY (new).

### Lessons

- Key aliases must be unique. An accidental run blocked reruns.
- "Delete Knowledge Base Key" deletes by `key_aliases`.
- "Make Knowledge Base Key" ends with Get Dictionary Value (error.message) plus Show Alert.

## Next single action

Finish the test. AURA isn't connected, so I'm testing the proxy directly with a shortcut, **Knowledge Base embed test**, a copy of "ANLLMS test".

- URL: https://litellm-proxy-700813965617.us-east1.run.app/embeddings
- Body fields: model = gemini-embedding-001, input = hello (Text), dimensions = 768 (Number). All other fields deleted.
- Still needed: Authorization header = "Bearer " plus the new key, copied from Infisical. End with Get Dictionary Value (key `model`) plus Show Alert. A model name means success. An empty alert means we check error.message.
- I was asked to report the body field names and types, and the action names after the request. I haven't yet.
- Then: warn before running, run once, and afterward clear the key from the shortcut and the clipboard.

## After the test passes

1. Delete the old LITELLM_MASTER_KEY secret in Supabase by hand, because the sync won't delete it.
2. Check which Supabase secrets (the four R2 ones, LITELLM_BASE_URL, MCP_ACCESS_TOKEN) are missing from Infisical, names only. Supabase never shows values, so any missing R2 values come from their original source, such as Cloudflare. Anything in `/knowledge-base`, including subfolders, will sync to Supabase.
3. Optional: a reminder about a week before the token expires.
4. Continue the hunt: Working Copy files, and the leftover LITELLM_API_KEY and LITELLM_BASE_URL Actions secrets in the anllms GitHub repo.

## Other open items (from the October 15 handoff)

Rate-limit check on AURA's key (20 rpm). Decide on Render auto deploy. Optional ANLLMS_MODEL switch to a Gemini name. Heading scheme (h3/h6 vs h5/h6) and "ordered lists (ul)" wording. Where user settings live and when to add accounts. First MCP connector. Legal entity name for the copyright line. Confirm README.md, LICENSE, and AURA-BUILD-PLAN.md are committed. Optional cleanup: workflow path filter `.yml` to `.yaml`, delete the empty Infisical project "LiteLLM", consider Secret Manager. Later: AURA's Cloudflare function reads AURA_PROXY_KEY from its own secret store.
