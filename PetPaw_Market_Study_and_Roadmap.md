# PetPaw — PRD Review, Market Study & Roadmap

*Prepared 17 Nov 2026. Builds on `PetPaw_ProjectPlan.md`; it doesn't replace it.*

---

## 0. TL;DR

- **The market is real but crowded.** Plenty of apps already handle records and reminders for dogs and cats (11pets, PetNoter, PetDesk, Pawmi, Pett…). Many small exotic-pet apps launched in 2026 (Vivarium, Crittora, Scuttle, ExoCare…), but each covers only one niche.
- **The gap:** no app does **every species well in one place**. Generalist apps put dogs and cats first. Exotic apps are small indie tools with few ratings. A household with a dog *and* a gecko *and* a parrot has nowhere good to go.
- **Recommended positioning:** *"The care record for every animal you love — shared with everyone who looks after them."* Make three things stand out: **(1) care that adapts to each species, (2) shared care with family, sitters and vets, (3) record entry that takes almost no typing (AI import, quick-log).**
- **Remove social, messaging, marketplace and adoption from the first year.** Each needs network effects or moderation, or is a separate business. These features would delay launch and wouldn't help retention.
- **Recommended stack:** Expo (React Native + TypeScript) for iOS, Android and web from one codebase, plus Supabase (Postgres, EU region) and RevenueCat. Details in §5.
- **Roadmap:** Validate (≈1 month) → MVP closed beta (≈3 months) → public launch with shared care and freemium (≈2 months) → smart features (≈4 months) → ecosystem (vets, breeders, lost & found). Details in §6.

---

## 1. Review of the original plan

### What still holds
- The **core MVP is right in spirit**: pet profiles, health tracker, reminders and journal are exactly where every successful competitor started.
- A **modular approach** (adding features over time) suits a species-rich product.
- The suggested tools (React Native, Firebase/Postgres, FCM, GitHub Actions, Figma) are all still reasonable.

### What's missing or outdated

| Gap | Why it matters | Fix |
|---|---|---|
| **No problem statement, target user or differentiation** | The plan lists features but never says *why someone would switch from 11pets or a notes app* | Define personas and a positioning statement (§4.1) |
| **Dog/cat-shaped data model** ("breed, age, medical info") | A snake has no breed-based vaccine schedule. A reef tank needs water parameters. A horse needs a farrier. | Use a **species-agnostic core plus species templates** (§5.1). This is the most important architecture decision. |
| **No sharing or multi-user concept** | Pets are cared for by households, sitters, walkers and vets. Shared care is a top retention driver and a free growth loop. | Households, roles and sitter links (§4.2) |
| **No business model** | Affects which features are free and which tech is needed (RevenueCat, web payments) | Freemium with a generous free tier (§4.4) |
| **Social, messaging and marketplace treated as natural next steps** | Each is a different product with cold-start, moderation and legal problems. Instagram already wins pet social. | Move them to "later, only if the data says so" (Phase 5) |
| **No privacy, legal or compliance section** | GDPR applies to owner data, location and photos. Apple requires in-app account deletion. EU microchip rules arrive in 2028. | §4.5 |
| **No offline, localisation or accessibility** | Barns, vet waiting rooms and travel often have poor signal. You're Portugal-based, so PT/EN from day one is cheap now and expensive later. | Build offline-tolerant reminders, use i18n from day one, check accessibility |
| **Tech stack is a menu, not a decision** | Leaving it open slows the start | Pick one stack (§5) |
| **MongoDB suggested as the default** | Health records, households and permissions are relational data | Postgres (with JSONB for species-specific fields) |
| **Steps out of order** ("Set Up Database" after "Build Core Features"; no user research) | Retrofitting data models is costly | Validation → data model → build (§6) |
| **No success metrics** | You can't tell if the MVP works | KPIs per phase (§7) |

### Note on the name
"**Paw**Paw" suggests dogs and cats. If the pitch is "every animal", check that the name doesn't undercut it: birds, fish and snakes have no paws. Also search the App Store, Google Play and EUIPO/INPI trademarks for "PetPaw" before you invest in branding. The pet category has many similar names.

---

## 2. Market overview

### Size and growth
- **Pet care apps:** about **$1.2B–$2.7B (2025)**, growing **5–7%/yr**. Estimates vary with how firms define the category ([OMR Global](https://www.omrglobal.com/industry-reports/pet-care-apps-market), [OG Analysis](https://www.marketresearch.com/OG-Analysis-v3922/Pet-Care-Apps-Outlook-Share-42886000)).
- **Broader pet tech:** about **$15.6B (2025)** at about **12% CAGR** ([GMI](https://gminsights.com/industry-analysis/pet-tech-market)).
- **US:** **95M households** own a pet. The industry reached **$158B in 2025** ([APPA](https://americanpetproducts.org/news/u.s.-pet-industry-reaches-158-billion-in-2025-poised-for-continued-growth-in-2026)).
- **Europe:** about **139M households** and **about 300M pets** ([FEDIAF 2025](https://europeanpetfood.org/_/news/fediaf-publishes-2025-facts-and-figures/)).

### The "beyond dogs and cats" opportunity
- US: about **6M households with birds**, about **6M with small animals**, about **6M with reptiles** and about **2M with horses** ([APPA](https://americanpetproducts.org/news/from-bigger-tanks-to-stronger-bonds-fish-reptile-ownership-evolves-in-2025)).
- **Gen Z is driving reptile, bird and small-animal growth** ([APPA research](https://americanpetproducts.org/blog/research-shows-gen-z-fuels-reptile-growth-while-millennials-lead-fishkeeping), [PetfoodIndustry](https://www.petfoodindustry.com/pet-ownership-statistics/news/15773381/report-gen-z-drives-bird-small-animal-ownership-growth)). These users are young, mobile-first and willing to pay for good tools.
- Exotic keepers **track far more than dog owners**: feeding and refusals, sheds, weights, enclosure temperature and humidity, UVB bulb age, and breeding. Their care is data-heavy, so a good tracker matters more to them.

### Regulation creating demand (Europe)
- **New EU regulation on dog and cat welfare and traceability**, approved by Parliament on 28 Apr 2026. **All dogs and cats must be microchipped and registered in interoperable national databases**, and the rules apply from **2028**. Owners travelling into the EU must pre-register online ([FVE](https://uevp.fve.org/news/eu-adopts-landmark-rules-on-pet-welfare-and-traceability), [GlobalPetIndustry](https://globalpetindustry.com/news/eu-new-pet-laws-strengthen-traceability-welfare-breeding-and-cross-border-travel/)).
- **Portugal:** SIAC is mandatory for dogs, cats and ferrets. The gov.pt app now shows a **digital DIAC** (microchip number and rabies vaccine) but nothing else: no reminders, no other species, no history ([Agroportal/DGAV](https://www.agroportal.pt/documento-de-identificacao-do-animal-de-companhia-ja-disponivel-na-app-gov-pt/)).
- → **Opportunity:** be the owner-friendly layer on top of official records: microchip, passport, rabies validity, a travel checklist and "what's legally required for my pet in my country".

---

## 3. Competitive landscape

| Segment | Examples | Strengths | Weaknesses → your opening |
|---|---|---|---|
| **Owner-managed records** | 11pets, PetNoter, Pawmi, PokiPaw, Pet Care Tracker, Pett | Mature record-keeping, reminders. PetNoter and Pawmi add AI (record Q&A, PDF parsing). | Dog/cat-first. Pawmi is dogs and cats only. 11pets puts **reminders, a second pet and family members behind Premium**, which annoys users. |
| **Clinic-connected** | PetDesk, VitusVet | Records flow in automatically from the vet | Only works if *your* vet uses them. Mostly US. Weak for exotics. |
| **Hardware companions** | Tractive, Petcube, Whistle | GPS and health data, sticky subscriptions | The app is secondary to the device. **No official public APIs** (Tractive integrations are community-built). |
| **Exotic niche (2026 wave)** | Vivarium, Crittora, Scuttle Pet, ExoCare, Lizaro, ReptiTrack, Exotic Bird Care | Deep species features (sheds, feeding, enclosure) | Indie, new and with few ratings. Single-niche, so a reptile app is useless for your dog. Paid tiers are €7–8/mo. |
| **AI symptom checkers / telehealth** | Pet Vet 247, PetCare AI, FirstVet, Vetster | High-intent moments | Accuracy is questionable. One unverified test found the right condition in the top 5 only about 60% of the time. Liability and regulatory risk. |
| **Government** | gov.pt digital DIAC (PT) | Official, trusted | Read-only, minimal, dogs/cats/ferrets only |

Sources: [PetNoter guide](https://petnoter.com/best-pet-care-apps/), [Pawmi comparison](https://www.getpawmi.com/best-pet-health-record-apps), [AnimalDossier](https://animaldossier.com/guides/best-pet-health-apps-2026), [Petiogo](https://www.petiogo.com/blog/best-pet-care-apps-2026), [11pets on App Store](https://apps.apple.com/app/id1232470530), [Vivarium](https://apps.apple.com/app/id6765792488), [Crittora](https://apps.apple.com/app/id6762208020), [Scuttle Pet](https://apps.apple.com/app/id6748359004), [Exotic Bird Care](https://apps.apple.com/ca/app/id6760437609), [Pet Vet 247](https://apps.apple.com/app/id6760194794), [Home Assistant Tractive](https://www.home-assistant.io/integrations/tractive/). Many comparison guides are written by competing app makers, so treat their rankings as biased.

### What users complain about
These are indicative signals; no solid public retention study exists for pet apps.
- **Feature overload.** Owners prefer simple and reliable over packed with features. In a 2025 survey of about 1,900 European owners, **≈79% preferred a simple, reliable device to a feature-heavy one, and about 60% refused to pay extra for features** ([Weenect](https://www.weenect.com/us/en/press/pet-tech/)). That survey covers devices, but the lesson applies to apps.
- **Login/account problems, subscription pricing, reliability bugs and support** are the largest app-store complaint categories ([applecharts roadmap](https://www.applecharts.com/roadmap/pet+care+app+by+animal+id)).
- **Data-entry fatigue.** Every records app dies when users stop typing things in, which is why AI import and quick-log matter.

### Key takeaways
1. **Records and reminders are table stakes**, not differentiation. You must do them *better* (faster entry, reliable reminders) and **not paywall the basics**.
2. **The winning wedge is breadth with depth:** generalist coverage *with* real species-specific tools.
3. **Shared care is under-served.** Pett does family feeding, and most others charge for it.
4. **Europe/Portugal is under-served** compared with the US. Localisation and EU travel/microchip features are cheap advantages.
5. **AI should reduce typing, not diagnose.** Document import is safe, high-value and proven (Pawmi, PetNoter). Symptom diagnosis carries liability.

---

## 4. Product recommendations

### 4.1 Positioning and target users

**Positioning:** *Every animal, every caretaker, one record.*

**Primary personas (MVP):**
1. **Multi-pet household:** e.g. a dog plus a cat plus a rabbit, with 2+ adults sharing care. Needs reminders, "has the cat been fed?" and vet history.
2. **Exotic keeper:** reptiles or birds, often Gen Z or millennial, data-hungry, active in online communities. Needs feeding/shed/weight/enclosure logs.
3. **Dog/cat owner who travels or uses sitters:** needs vaccine and passport status and a sitter handover.

**Secondary (later):** horse owners, aquarium keepers, small breeders, shelters, pet sitters (B2B).

### 4.2 Features that would add the most value (beyond the original PRD)

| # | Feature | Why | Phase |
|---|---|---|---|
| 1 | **Species templates** (dog, cat, rabbit/small mammal, bird, reptile → later horse, fish/aquarium, invertebrates, "other") with species-specific trackers | The core differentiator | MVP |
| 2 | **Quick-log** (one-tap "fed / walked / meds given / shed / weighed", plus home-screen widgets) | Beats data-entry fatigue and drives daily use | MVP (widgets in Phase 3) |
| 3 | **Reliable recurring reminders** (local notifications that work offline, with snooze and "done") | The #1 reason people install. Must never fail. | MVP |
| 4 | **Vet-ready PDF / share link** (health summary, vaccines, meds, allergies, weight chart) | Immediately useful at the vet. Spreads the brand to vets. | MVP |
| 5 | **Household and caretaker sharing** (owner / co-owner / sitter / vet read-only roles) and a live "done" status | Retention and growth loop (every invite is a new user) | Phase 2 |
| 6 | **Sitter mode**: a temporary link with care instructions, schedule and emergency contacts that expires automatically | Solves a real, recurring pain. Strong virality. | Phase 2 |
| 7 | **Emergency card and QR pet profile** (public page: name, photo, "I'm lost, call…", medical needs). Works with any QR tag or sticker. | Lost & Found without needing network density. Possible physical-tag revenue. | Phase 2 |
| 8 | **Expense tracker** (vet, food, insurance, by pet, exportable) | Frequently requested. Helps with insurance claims. | Phase 2 |
| 9 | **AI document import**: photo or PDF of a vaccine card, invoice or lab result → structured records and suggested reminders | Removes the biggest friction: getting history *in* | Phase 3 |
| 10 | **Trends and insights**: weight curves, feeding-refusal streaks, overdue care, with gentle "talk to your vet" nudges | Turns logs into value without diagnosing | Phase 3 |
| 11 | **Travel and compliance (EU-first)**: microchip, passport, rabies validity, a country checklist, the 2028 EU registration reminder | Unique in Europe. Regulatory tailwind. | Phase 3 |
| 12 | **Species care guides** (curated or vet-reviewed, linked to templates) | SEO and onboarding value. Helps first-time exotic owners. | Phase 3 |
| 13 | **Pet ownership transfer** (breeder/shelter → new owner, with full history) | A B2B growth loop: every adoption onboards a new user | Phase 4 |
| 14 | **Vet / clinic portal** (owner-consented access and record upload) | Long-term moat, possible B2B revenue | Phase 4 |
| 15 | **Device integrations** (GPS, smart feeders, scales), partner-led | Lower priority. APIs are closed, so pursue partnerships, not scraping. | Phase 4+ |

**Keep from the original PRD:** profiles, health tracker, reminders, journal. Simplify **Gallery** into photos attached to pets and events, not a separate social gallery.

**Postpone:** social feed, followers, messaging, events, marketplace, adoption, services directory (Phase 5, only if users ask for them).

### 4.3 Species template examples

| Species group | Specific trackers / fields |
|---|---|
| Dog | Walks/activity, vaccines (core + local), flea/tick/worming cycle, grooming, training notes |
| Cat | Vaccines, worming, litter/appetite notes, indoor/outdoor, dental |
| Rabbit / small mammal | Vaccines (RHD/myxo), nail trims, diet (hay/pellets), weight trend (critical) |
| Bird | Weight (grams, frequent), molt, wing/nail/beak trims, diet, cage cleaning, band ID |
| Reptile | Feeding (prey type/size, accepted/refused), shed (complete/incomplete), weight/length, enclosure temp/humidity, UVB bulb change date, brumation, breeding |
| Fish / aquarium *(Phase 3)* | Tank as the "pet", water parameters (pH, ammonia, nitrite, nitrate, temp), water changes, livestock list |
| Horse *(Phase 3)* | Farrier, dental, worming, vaccines, Coggins/passport, feed, stable/livery contact |
| Other | Generic weight, feeding, notes, custom fields |

### 4.4 Business model
- **Freemium. Don't paywall what competitors get criticised for paywalling.**
  - **Free:** up to about 3 pets, full records, unlimited reminders, quick-log, one co-owner, PDF export.
  - **Premium (≈ €3.99/mo or €29.99/yr, test this):** unlimited pets, unlimited caretakers and sitter links, AI document import, advanced charts and insights, document storage beyond a quota, custom fields, travel module.
- **Trial length:** RevenueCat data suggests **longer trials convert better** (≈42.5% median for 17+ day trials vs ≈25.5% for ≤4 days). The median trial-to-paid rate for health & fitness in 2025 was ≈40% ([RevenueCat blog](https://www.revenuecat.com/blog/growth/free-trial-length), [Athletech](https://athletechnews.com/?p=124212)). Test a 14-day trial.
- **Later revenue:** QR tags and stickers (physical product), insurance and food affiliate partnerships (transparent, opt-in), B2B plans for breeders, shelters, sitters and clinics.
- **Avoid ads.** They undermine trust in a health app.

### 4.5 Privacy, legal and trust (non-negotiable)
- **GDPR:** host data in the EU, write a privacy policy, offer data export (JSON/PDF) and in-app **account deletion** (also required by Apple), collect consent for analytics, and minimise location data.
- **Medical disclaimers** on any AI or insight feature: "not veterinary advice".
- **App Store privacy labels / Play data-safety form** filled in accurately.
- **Public QR pages:** owner controls exactly what is shown. No home address by default.
- **Backups and reliability:** losing a pet's medical history destroys trust. Use automated DB backups and test restores.

---

## 5. Recommended tech stack

| Layer | Choice | Why |
|---|---|---|
| **App (iOS + Android + web)** | **Expo (React Native + TypeScript)** with Expo Router | One codebase for all three targets, including web. Largest hiring pool. EAS Build/Submit and **over-the-air updates**. A good fit for a form- and list-heavy app. *(Flutter is a fine alternative if you already know Dart, but its web output is weaker.)* |
| **UI** | NativeWind (Tailwind for RN) + a component kit (e.g. React Native Reusables / gluestack), **Figma** for design | Fast and consistent, and shares design tokens with the web |
| **State and data fetching** | TanStack Query (with persisted cache) + Zustand for local UI state | Caching, retries and offline-tolerant reads |
| **Forms and validation** | React Hook Form + **Zod** | Zod schemas also validate species-template JSON on client and server |
| **Backend** | **Supabase** (Postgres, Auth, Storage, Edge Functions, Row-Level Security), **EU region** | Relational data fits records, households and permissions. RLS enforces sharing rules in the DB. Open-source, so less lock-in. Predictable pricing. |
| **Offline** | MVP: persisted cache + **local notifications** for reminders. Phase 3: **PowerSync** (Postgres ↔ SQLite sync) if full offline editing is needed | Reminders must work with no signal from day one. Full sync can wait. *(If offline-first is the top priority from day one, Firebase/Firestore is the main alternative.)* |
| **Auth** | Supabase Auth: Sign in with Apple, Google, email magic link | Fewer password problems. Account issues are a top complaint category. |
| **Notifications** | `expo-notifications`: **local** for reminders, **Expo Push (APNs/FCM)** for household events | Local = reliable and free. Push = "Ana fed Bolinhas". |
| **Payments** | **RevenueCat** (App Store + Play + Stripe for web) | Handles receipts, trials, entitlements and analytics across platforms |
| **AI (Phase 3)** | Vision-capable LLM via a **server-side** Edge Function (e.g. Claude Sonnet for document extraction, Haiku for cheap classification) returning Zod-validated JSON. User confirms before saving. | Never call the LLM from the client. Keep a human-in-the-loop for medical data. |
| **Analytics and flags** | **PostHog** (EU cloud) | Product analytics, funnels, feature flags and A/B tests (paywall) in one tool |
| **Crash reporting** | **Sentry** | Industry standard, with good Expo support |
| **i18n** | i18next / `expo-localization`, **PT-PT + EN** from day one | Cheap now, painful later |
| **Testing** | Jest + React Native Testing Library, **Maestro** for E2E flows | Maestro is the simplest mobile E2E option |
| **CI/CD** | GitHub Actions + EAS (build, submit, update channels) | Automated store builds and staged OTA rollouts |
| **Marketing site and public pet pages** | Astro or Next.js (SEO for species guides) + QR profile pages, hosted on Vercel/Netlify/Cloudflare | Species care guides drive organic traffic |

### 5.1 Data model: the key decision
Design for **any species from day one**, even if only 5 templates ship first:

```
users ── household_members ── households ── pets
                                             │  species_group, species, template_version,
                                             │  name, sex, birth/hatch date (or estimate),
                                             │  microchip, identifiers (band, passport…), photo
                                             │  attributes JSONB  ← species-specific fields
                                             │
                     ┌───────────────────────┼─────────────────────────┐
                 events / logs          reminders                documents
            type (feeding, shed,     rrule recurrence,        file, kind (vaccine
            vaccine, weight, vet…),  next_due, assignee,      card, invoice, lab),
            occurred_at, by_user,    linked event type        extracted_data JSONB
            payload JSONB (Zod-
            validated per type)
                     │
              measurements (weight, length, temp, humidity, water params…)
              value + unit (store SI, display user's units)
```

- **Species templates live in config (JSON/TS), not in code branches.** A template declares fields, log types, default reminders and units. Adding "axolotl" should be a config change, not a release.
- Use **RRULE** (iCal recurrence) for reminders. It handles "every 3 weeks", "every 1st of month" and "every 5–7 days for feeding".
- Store **who did what** (`by_user`) from the start. Shared care depends on it.
- Use **units and estimates** throughout: grams vs kg, exact vs estimated birth or hatch date (common for rescues and exotics).

---

## 6. Roadmap

Durations assume **1–2 developers**, part-time-to-full-time. Adjust to your team.

### Phase 0 — Validate and design (≈ 3–5 weeks)
- [ ] **Interview 15–20 owners**: about 8 dog/cat, about 5 reptile/bird, about 2 horse/aquarium, plus 1–2 vets and 1–2 pet sitters. Ask: *what do you track today, where, and what went wrong?*
- [ ] **Teardown of competitors**: use 11pets, PetNoter, Pett, Vivarium and Crittora for a week each. Note what's paywalled and where friction is.
- [ ] Final **positioning, name and trademark check**, then a landing page with a waitlist (PT/EN) to test messaging.
- [ ] **Wireframes** of the core flows: onboarding → add pet → log → reminder → vet PDF.
- [ ] **Data model and species templates v1**. Decide the stack (§5).
- [ ] Repo, CI, Expo + Supabase skeleton, i18n, Sentry and PostHog wired in.

**Exit criteria:** 50+ waitlist sign-ups, with the top 5 pains confirmed in interviews.

### Phase 1 — MVP, closed beta (≈ 10–12 weeks)
- [ ] Auth (Apple, Google, email). Onboarding in under 60 seconds to the first pet.
- [ ] Pet profiles with **templates: dog, cat, rabbit/small mammal, bird, reptile, other**.
- [ ] Health records: vaccines, medications, vet visits, allergies/conditions, weight, documents and photos.
- [ ] **Quick-log** and journal timeline per pet.
- [ ] **Recurring reminders** (local notifications, snooze, mark done, overdue state).
- [ ] Species-specific logs for the first templates (feeding/shed/enclosure for reptiles, molt/weight for birds…).
- [ ] **Vet-ready PDF export**.
- [ ] Settings: units, language (PT/EN), data export, account deletion.
- [ ] Beta via TestFlight and Play internal testing with 30–100 users from the waitlist and exotic communities.

**Exit criteria:** ≥40% of beta users still active at week 4, reminders working reliably (<1% missed), and qualitative "I'd be upset if this went away".

### Phase 2 — Public launch: shared care and monetisation (≈ 8–10 weeks)
- [ ] **Households and roles** (owner, co-owner, caretaker, vet read-only) and push notifications for shared events.
- [ ] **Sitter mode** (expiring link, instructions, schedule, emergency contacts).
- [ ] **Emergency card and QR pet profile page** (web).
- [ ] **Expense tracker**.
- [ ] **Freemium and paywall** via RevenueCat, with A/B tests on trial length and price.
- [ ] App Store / Play listing optimisation in PT and EN, launch in PT + one or two EN markets (UK/IE), and outreach to reptile and bird communities.
- [ ] Web app (Expo web) for record viewing and editing.

**Exit criteria:** D30 retention ≥20%, ≥25% of active households with 2+ members, first paying users.

### Phase 3 — Smart and deep (≈ 3–4 months)
- [ ] **AI document import** (photo/PDF → records and reminders, with user confirmation).
- [ ] **Insights and trends** (weight curves, feeding-refusal streaks, overdue care).
- [ ] New templates: **horse, fish/aquarium, invertebrates**, plus custom fields.
- [ ] **Travel and EU compliance module** (passport, rabies validity, country checklists, 2028 registration).
- [ ] Species care guides (app plus SEO site).
- [ ] Home-screen widgets, Apple Watch / Wear OS quick-log.
- [ ] Full offline editing (PowerSync) if beta data shows it's needed.

### Phase 4 — Ecosystem (6–12 months)
- [ ] **Pet transfer** for breeders and shelters (B2B pilot: 3–5 shelters or breeders in Portugal).
- [ ] **Vet/clinic portal** (consented sharing, record upload).
- [ ] **Lost & Found network** built on QR scans, plus local alerts once user density allows.
- [ ] Partner integrations (GPS trackers, smart feeders, scales), insurance and food partnerships.
- [ ] Expand to ES/FR/DE/BR.

### Phase 5 — Community (only if data supports it)
- Social feed, followers, events, services directory, adoption, marketplace. Revisit based on what users ask for and whether you have the density and moderation capacity.

---

## 7. KPIs to track from day one

| Stage | Metric | Target (initial) |
|---|---|---|
| Activation | % of new users who add a pet **and** create a reminder or log in the first session | ≥ 60% |
| Engagement | Logs and completed reminders per active pet per week | ≥ 3 |
| Retention | D1 / D7 / D30 | 40% / 25% / 15–20% |
| Shared care | % of households with ≥2 members | ≥ 25% |
| Reliability | Missed or duplicate reminder reports | < 1% |
| Monetisation | Install → trial; trial → paid | ≥ 8%; ≥ 35% |
| Species mix | % of pets that aren't dogs or cats | Watch this. It validates the wedge. |

---

## 8. Main risks and mitigations

| Risk | Mitigation |
|---|---|
| **Crowded market:** AI-assisted dev means many new pet apps ship every month | Compete on breadth plus species depth plus shared care plus Europe, not on records alone. Build community trust in exotic niches early. |
| **Scope creep** (the original PRD has about 20 features) | Ruthless phase gates. Nothing from Phase 5 before Phase 2 metrics are hit. |
| **Data-entry fatigue → churn** | Quick-log, widgets, AI import, smart default reminders per species |
| **Reminder unreliability** (OS notification limits, permissions) | Local scheduling, a permission-education screen, an in-app "upcoming" list as a fallback, and monitoring |
| **AI/medical liability** | No diagnosis. AI only extracts and summarises, with user confirmation and disclaimers. Vet-reviewed species content. |
| **Species templates done badly** (exotic keepers are experts and will notice) | Co-design templates with experienced keepers and an exotic vet. Beta in their communities first. |
| **GDPR / trust incident** | EU hosting, RLS tested, minimal data, backups, clear privacy page |

---

## 9. Immediate next steps (this week)
1. Decide team size and time budget. That sets the roadmap durations.
2. Confirm the **positioning** and the **5 MVP species templates**.
3. Do the **name and trademark check** for "PetPaw".
4. Write the interview script and book the first 5 interviews (include at least 2 exotic keepers).
5. Install and test 11pets, PetNoter, Pett and Vivarium. Record the friction points.
6. Set up the repo: Expo + TypeScript + Supabase (EU) + i18n + Sentry + PostHog + GitHub Actions.

---

### Sources
- Market: [OMR Global](https://www.omrglobal.com/industry-reports/pet-care-apps-market) · [OG Analysis](https://www.marketresearch.com/OG-Analysis-v3922/Pet-Care-Apps-Outlook-Share-42886000) · [GMI pet tech](https://gminsights.com/industry-analysis/pet-tech-market) · [APPA 2025 industry](https://americanpetproducts.org/news/u.s.-pet-industry-reaches-158-billion-in-2025-poised-for-continued-growth-in-2026) · [APPA fish & reptile](https://americanpetproducts.org/news/from-bigger-tanks-to-stronger-bonds-fish-reptile-ownership-evolves-in-2025) · [APPA Gen Z reptiles](https://americanpetproducts.org/blog/research-shows-gen-z-fuels-reptile-growth-while-millennials-lead-fishkeeping) · [PetfoodIndustry Gen Z](https://www.petfoodindustry.com/pet-ownership-statistics/news/15773381/report-gen-z-drives-bird-small-animal-ownership-growth) · [FEDIAF 2025](https://europeanpetfood.org/_/news/fediaf-publishes-2025-facts-and-figures/)
- Regulation: [FVE — EU adopts pet welfare & traceability rules](https://uevp.fve.org/news/eu-adopts-landmark-rules-on-pet-welfare-and-traceability) · [GlobalPetIndustry — EU pet laws](https://globalpetindustry.com/news/eu-new-pet-laws-strengthen-traceability-welfare-breeding-and-cross-border-travel/) · [RTÉ](https://www.rte.ie/news/world/2026/0428/1570601-eu-laws-cats-dogs/) · [Agroportal — DIAC in gov.pt](https://www.agroportal.pt/documento-de-identificacao-do-animal-de-companhia-ja-disponivel-na-app-gov-pt/) · [Pplware — SIAC](https://pplware.sapo.pt/internet/siac-e-obrigatorio-registar-online-os-seus-animais-de-estimacao/)
- Competitors: [PetNoter](https://petnoter.com/best-pet-care-apps/) · [Petiogo](https://www.petiogo.com/blog/best-pet-care-apps-2026) · [AnimalDossier](https://animaldossier.com/guides/best-pet-health-apps-2026) · [Pawmi](https://www.getpawmi.com/best-pet-health-record-apps) · [11pets](https://apps.apple.com/app/id1232470530) · [Petcube Care](https://petcube.com/support/care/) · [Vivarium](https://apps.apple.com/app/id6765792488) · [Crittora](https://apps.apple.com/app/id6762208020) · [Scuttle Pet](https://apps.apple.com/app/id6748359004) · [ExoCare](https://apps.apple.com/us/app/-/id6764813422) · [Exotic Bird Care](https://apps.apple.com/ca/app/id6760437609) · [Pet Vet 247](https://apps.apple.com/app/id6760194794) · [AI symptom checker review](https://blogstarterhub.pages.dev/pets/ai-pet-health-apps-detect-illness-early-2026) · [Tractive / Home Assistant](https://www.home-assistant.io/integrations/tractive/)
- User sentiment: [Weenect survey](https://www.weenect.com/us/en/press/pet-tech/) · [applecharts complaints](https://www.applecharts.com/roadmap/pet+care+app+by+animal+id) · [MeasuringU pet benchmark](https://measuringu.com/pet-benchmark-2025/)
- Tech & monetisation: [RevenueCat trial length](https://www.revenuecat.com/blog/growth/free-trial-length) · [Athletech — RevenueCat 2025](https://athletechnews.com/?p=124212) · [RN vs Flutter 2026](https://pasqualepillitteri.it/en/news/8279/react-native-2026-stack-choices-vs-flutter) · [Supabase vs Firebase 2026](https://www.bytebase.com/blog/supabase-vs-firebase.md) · [Supabase vs Firebase (offline)](https://dev.to/pockit_tools/supabase-vs-firebase-in-2026-the-honest-comparison-after-using-both-in-production-3e5)

*Caveat: many 2026 comparison articles come from competing app makers. Market-size reports disagree widely. Competitor prices change often. Verify the figures before using them in a pitch deck.*
