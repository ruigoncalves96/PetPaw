# Project Status & Handoff

*Last updated: 21 Nov 2026. Read this first in a new session.*

## The project
A cross-platform app (iOS, Android, maybe web) for pet owners to manage everything about their animals, for **every species** (dogs and cats first, but also rabbits, birds, reptiles, fish, horses…). The working name is "PetPaw", but the final name is **not decided**, so respondent-facing material is kept brand-neutral. The owner is based in Portugal, so the content is PT-PT + EN.

## Key documents
| File | What it is |
|---|---|
| [PetPaw_Market_Study_and_Roadmap.md](PetPaw_Market_Study_and_Roadmap.md) | Review of the old PRD, market study, competitors, recommended features, tech stack, 5-phase roadmap, KPIs, risks |
| [brand/palette-directions.html](brand/palette-directions.html) | Brand round 1: 4 colour directions (Moss & Marigold, Lagoon & Coral, Plum & Apricot, Cobalt & Sun) with WCAG checks, light and dark mockups, shared status and species colours. Published at https://claude.ai/artifact/Ryc3aVJvTo7QboCnW437NY |
| [brand/Brand_Foundation.md](brand/Brand_Foundation.md) | **Brand Foundation v1** (21 Nov 2026): soul "No animal forgotten", purpose, values, positioning, personality, voice, visual principles, naming brief, open decisions (§10). Built from the founders' answers to [brand/Brand_Foundation_Questions.md](brand/Brand_Foundation_Questions.md) (the answers file was not kept) |
| [research/Competitor_PetNoter.md](research/Competitor_PetNoter.md) | PetNoter teardown: store stats, review mining (complaint/praise themes), implications, hands-on checklist |
| [research/PetPaw_Survey_Tally.md](research/PetPaw_Survey_Tally.md) | Full survey build sheet (EN/PT), logic, codebook, analysis plan, launch checklist. **Kept in sync with the live forms.** |

## Decisions made
- **Positioning:** "Every animal, every caretaker, one record." Stand out on (1) species-specific care, (2) shared care (family/sitters/vets), (3) almost no typing (quick-log, later AI document import).
- **Postponed:** social feed, messaging, marketplace, adoption (Phase 5, only if the data supports them).
- **Recommended stack (not started yet):** Expo (React Native + TypeScript) for iOS/Android/web, Supabase (Postgres, EU region), RevenueCat, PostHog (EU), Sentry, i18n PT/EN. Data model: a species-agnostic core plus species templates defined in config.
- **Business model:** freemium, with reminders, records and sharing basics free. Premium is about €3.99/mo or €29.99/yr (to be tested).

## Roadmap position: Phase 0 (Validate & design)
- [x] PRD review, market study, roadmap
- [x] Survey designed, built in Tally, and **published** (18 Nov 2026)
- [x] Survey **finished and ready to share** (18 Nov 2026): exclusive options, B5 removed, WhatsApp share link on the thank-you page
- [x] Connect Google Sheets to both forms (owner must sign in to Google in Tally → Integrations)
- [x] Send one test response per form, check that `lang` / `src` / `species_count` arrive, then delete the test rows
- [ ] Pilot with about 5 people (including 2 exotic keepers)
- [x] Distribute with tracking links (keep a list of every `src` used)
- [ ] Analyse at 25–50 responses, then at 100+ (Claude can read submissions via the Tally connector)
- [ ] Write the interview script for the 20-min calls (volunteers come from the survey's F1 question)
- [x] Survey being distributed (Reddit, Discord, friends), started 19 Nov 2026
- [x] Quick look at competitors by the owner: **PetNoter is the only serious one**, the rest feel cheap or clunky
- [x] PetNoter desk teardown and review mining (19 Nov 2026), see `research/Competitor_PetNoter.md`. About 10K+ Android downloads after 5 years, little iOS traction, no PT, generic species. Weak spots: reminders, paywall, reliability.
- [ ] PetNoter hands-on week (checklist in §6 of the teardown)
- [x] Remaining teardowns (11pets, Pett, Vivarium, Crittora). Optional, since the owner found them weak.
- [ ] Name and trademark check. Round 1 review of the founders' list (21 Nov 2026) is in `brand/Name_Review_Round1.md`: **Mieli** and **Arca** shortlisted; Kin, Gemma, Mena, Lana dropped. Round 2 longlist (21 Nov 2026) is in `brand/Name_Longlist_Round2.md`: shortlist of 8 (Omia, Nolia, Velia, Noa, Mieli, Arca, Abri, Nidi), with Claude's top 3 Omia / Nolia / Velia. Mieli also looks like *Miele*. Next: cold-read test with 5–10 people, then a TMview check (classes 9/42/44) on the final 2–3.
- [ ] Brand round 1: palette directions (19 Nov 2026). Recommended Moss & Marigold, alternative Plum & Apricot. **Owner to pick 1–2 and test with 5–10 people.**
- [x] Brand foundation v1 (21 Nov 2026), see `brand/Brand_Foundation.md`. Name "PetPaw" is being retired: no "pet", no "paw", soft round-sounding word that works in any language. **Founders to settle §10 open decisions**, then the naming round.
- [ ] Brand round 2: typography, then logo and app icon once the name is chosen, then a design system
- [ ] Wireframes, data model, repo skeleton

## Live survey (Tally, workspace "My workspace")
| | Form ID | Public link | Edit |
|---|---|---|---|
| English | `RWor84` | https://tally.so/r/RWor84 | tally.so/forms/RWor84/edit |
| Portuguese (PT-PT) | `X5oN8Y` | https://tally.so/r/X5oN8Y | tally.so/forms/X5oN8Y/edit |

- **Structure (both forms identical):**
  - 8 pages: intro → about you → current care → species-specific → what would help → paying → stay in touch → thank you
  - 52 logic rules (42 + 10 for exclusive options)
- **Tracking:**
  - Hidden fields `lang` and `src` come from the URL, e.g. `https://tally.so/r/X5oN8Y?lang=pt&src=fb_caes_pt`.
  - The calculated field `species_count` counts the *kinds* of animal ticked in A2 (all 10 options). It drives 4 HIDE rules (animal-count options and "most effort") and gives the multi-species segment (≥2). Keep it.
  - The thank-you page has a WhatsApp share link (`wa.me`) and a copy link, both tagged `src=friends` and the form's `lang`.
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
- Branch `research/owner-survey`, pushed to `origin`, **not merged** into `main` yet. Commits d50da0e → latest.
- `main` has the earlier commit "PRD" (market study).
