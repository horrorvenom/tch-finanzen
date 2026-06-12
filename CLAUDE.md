# TCH Finanz Manager — CLAUDE.md

## Project Overview
Single-page finance manager for **Touring Club Helvetia** (Swiss auto workshop + towing).  
Stack: plain HTML + CSS + JS in one file (`index.html`). No build step, no npm, no framework.  
Backend: Supabase (auth + DB). Falls back to `localStorage` in demo mode when Supabase is unreachable.

## Architecture Rules
- **One file only.** All HTML, CSS, JS stays in `index.html`. Do not split into separate files unless explicitly asked.
- **Dual data layer.** Every DB operation goes through `dbGet/dbIns/dbUpd/dbDel`. Never call `supabase` directly outside those helpers.
- **localStorage fallback must stay working.** Demo mode (`ceo@tch.ch / tch2026`) must work without Supabase.
- **Role gating.** All privileged actions (Mitarbeiter mgmt, Konto edit) must check `isCEO()`. Never skip this.

## Language & Formatting
- UI language is **German (Swiss)**. All labels, alerts, placeholders stay German.
- Currency: CHF. Format with `fmt(n)` which uses `de-CH` locale. Never output raw numbers to the UI.
- Dates: `fmtD(d)` — always use this helper, never raw `Date.toLocaleDateString` calls.

## Key Constants
| Constant | Value | Note |
|---|---|---|
| `CEO_EMAIL` | `alper.star3@gmail.com` | Determines CEO role on login |
| `SB_URL` | `https://rbnrppheuvbmnlidcmei.supabase.co` | Supabase project |
| `SB_KEY` | `sb_publishable_…` | Publishable key, safe to keep in frontend |

## Supabase Tables
- `einnahmen` — income records
- `ausgaben` — expense records
- `mitarbeiter` — employees
- `settings` — key/value store (`key='konto'` holds account balance)
- `profiles` — user profiles with `role` and `name`

## AI Feature
The AI chat calls `https://api.anthropic.com/v1/messages` with model `claude-sonnet-4-6`.  
The API key must be added as `'x-api-key': '<key>'` in the fetch headers — it is currently missing (feature will fail without it).

## Commit Policy
- **Commit after every meaningful change** with a clear message.
- **Always push to GitHub** (`git push origin main`) immediately after committing. Never leave commits local.
- Use Conventional Commits format: `feat:`, `fix:`, `style:`, `refactor:`, `docs:`.
- Subject line ≤ 50 chars. Body only when "why" is non-obvious.

## What NOT to do
- Do not introduce a build pipeline, bundler, or package.json.
- Do not split `index.html` into multiple files.
- Do not change `CEO_EMAIL` without understanding auth implications.
- Do not change currency formatting away from CHF/`de-CH`.
- Do not add English to the UI — all user-facing text must stay German.
