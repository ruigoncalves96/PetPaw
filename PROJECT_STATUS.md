# Project Status & Handoff

*Last updated: 18 Nov 2026. Read this first in a new session.*

## The project
A cross-platform app (iOS, Android, maybe web) for pet owners to manage everything about their animals, for **every species** (dogs and cats first, but also rabbits, birds, reptiles, fish, horses…). The working name is "PetPaw", but the final name is **not decided**, so respondent-facing material is kept brand-neutral. The owner is based in Portugal, so the content is PT-PT + EN.

## Key documents
| File | What it is |
|---|---|
| [PetPaw_Market_Study_and_Roadmap.md](PetPaw_Market_Study_and_Roadmap.md) | Review of the old PRD, market study, competitors, recommended features, tech stack, 5-phase roadmap, KPIs, risks |
| [research/PetPaw_Survey_Tally.md](research/PetPaw_Survey_Tally.md) | Full survey build sheet (EN/PT), logic, codebook, analysis plan, launch checklist. **Kept in sync with the live forms.** |

## Decisions made
- **Positioning:** "Every animal, every caretaker, one record." Stand out on (1) species-specific care, (2) shared care (family/sitters/vets), (3) almost no typing (quick-log, later AI document import).
- **Postponed:** social feed, messaging, marketplace, adoption (Phase 5, only if the data supports them).
- **Recommended stack (not started yet):** Expo (React Native + TypeScript) for iOS/Android/web, Supabase (Postgres, EU region), RevenueCat, PostHog (EU), Sentry, i18n PT/EN. Data model: a species-agnostic core plus species templates defined in config.
- **Business model:** freemium, with reminders, records and sharing basics free. Premium is about €3.99/mo or €29.99/yr (to be tested).

## Roadmap position: Phase 0 (Validate & design)
- [x] PRD review, market study, roadmap
- [x] Survey designed, built in Tally, and **published** (18 Nov 2026)
- [ ] Connect Google Sheets to both forms (owner must sign in to Google in Tally → Integrations)
- [ ] Send one test response per form, check that `lang` / `src` / `species_count` arrive, then delete the test rows
- [ ] Pilot with about 5 people (including 2 exotic keepers)
- [ ] Distribute with tracking links (keep a list of every `src` used)
- [ ] Analyse at 25–50 responses, then at 100+ (Claude can read submissions via the Tally connector)
- [ ] Write the interview script for the 20-min calls (volunteers come from the survey's F1 question)
- [ ] Competitor teardown (11pets, PetNoter, Pett, Vivarium, Crittora)
- [ ] Name and trademark check
- [ ] Wireframes, data model, repo skeleton

## Live survey (Tally, workspace "My workspace")
| | Form ID | Public link | Edit |
|---|---|---|---|
| English | `RWor84` | https://tally.so/r/RWor84 | tally.so/forms/RWor84/edit |
| Portuguese (PT-PT) | `X5oN8Y` | https://tally.so/r/X5oN8Y | tally.so/forms/X5oN8Y/edit |

- **Structure (both forms identical):**
  - 8 pages: intro → about you → current care → species-specific → what would help → paying → stay in touch → thank you
  - 42 logic rules
- **Tracking:**
  - Hidden fields `lang` and `src` come from the URL, e.g. `https://tally.so/r/X5oN8Y?lang=pt&src=fb_caes_pt`.
  - The calculated field `species_count` counts the species ticked.
- **Logic highlights:**
  - "No pets" jumps to the follow-up page. "Under 18" ends the form.
  - Species blocks are shown per animal ticked.
  - "How many animals" hides counts lower than `species_count`.
  - "Most effort" is visible by default and hidden if `species_count` < 2, and its options are hidden per species not ticked.
  - "Which app do you use?" is shown for "still use it" only if the earlier "Which app(s)" answer is empty.
  - Email and consent appear only when the person opts in.
- **Exclusive options** ("None of these", "Nobody", "Only me", "I don't really track it" in A4, B1, B3, D3, E1) use paired HIDE rules, because Tally has no built-in exclusive checkbox (added 18 Nov 2026, both forms).
- **Tally lesson learned:** a SHOW rule on a whole question re-shows *all* of its options and overrides per-option HIDE rules. Where options are filtered, use HIDE-only logic.
- **Every change must be made in BOTH forms**, keeping the options in the same order, and mirrored in the build sheet.
- The Tally tool results are very large (about 70k characters). Read the saved output with `jq` (`.context.feedback`, `.context.ledger`).

## Git
- Branch `research/owner-survey`, **not pushed and not merged** into `main`. Commits d50da0e → 82fcad3 (plus this status file).
- `main` has the earlier commit "PRD" (market study).
