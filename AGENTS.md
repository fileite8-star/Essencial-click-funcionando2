# AGENTS.md

## Project Context

This is a React/Vite photography website. Treat it as user-owned application code, keep changes focused on the user's request, and preserve existing project conventions.

Start with `README.md` for local setup, environment variables, and publish workflow.

## Key Files

- `src/`: frontend application source.
- `src/lib/supabase.js`: frontend Supabase client.
- `supabase/migrations/`: schema, row-level access policies, and media bucket.
- `supabase/functions/submit-quote/`: secure quote notification function.
- `vite.config.js`: Vite configuration and source alias.
- `.env.local`: local-only Supabase public configuration; never commit secrets.

## Working Notes

- Run `npm run dev` for the frontend.
- Set `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in `.env.local`.
- Apply the SQL migration and deploy `submit-quote` before enabling quote submissions.
- Set `RESEND_API_KEY` and `RESEND_FROM_EMAIL` as Edge Function secrets; never expose them to Vite.
- Run the relevant checks from `package.json` before finishing code changes.
