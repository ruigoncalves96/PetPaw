# Competitor teardown: PetNoter

*19 Nov 2026. Part 1 is desk research: store data plus **all** written reviews (≈44 on Google Play, ≈45 on the App Store). Part 2 is a hands-on checklist for you to fill in after using the app for a week.*

---

## 1. Snapshot

| | |
|---|---|
| **App** | PetNoter: Pet Health Tracker (`com.petnoter`, iOS id `1580581463`) |
| **Developer** | Individual developer account (Manu Benjamin). Support goes through a Gmail address. It looks like a **small indie team**. |
| **Launched** | Jul/Aug 2021, so **about 5 years on the market** |
| **Activity** | Very active: v2.3.5, updated 4 Nov 2026. The developer replies to almost every negative review and ships fixes. |
| **Google Play** | **10K+ downloads**, 4.4★, ≈158 ratings, ≈44 written reviews |
| **App Store** | **≈30 ratings worldwide** (17 US, 5 IN, 1 each in GB/IE/FR/ES/NL/IT, 0 in PT/BR/DE). Avg 4.2★ US. |
| **Claimed users** | Website: "30,000+ downloads". App Store text: "20,000+ pet parents". The stores show a smaller footprint (about 10K+ Android, with little iOS traction). |
| **Platforms** | Android and iOS. **No web app** (the site only has calculators). |
| **Languages (iOS)** | EN, ES, FR, DE, NL, CS, JA, KO, ZH. **No Portuguese.** |
| **Backend** | Firebase/Firestore (an error message quoted in reviews names `firestore`) |
| **Species** | Generic profiles for "dogs, cats, rabbits, birds and more". Tools are **dog/cat only** (weight/age calculators). **No species-specific trackers.** |
| **Pricing** | Free: **3 pets**, records, reminders, to-dos, weight, expenses. **Pro** (≈$29/yr per a Dec 2024 review, plus lifetime option): unlimited pets, AI assistant, documents, PDF export, family sharing (up to 5), vet contacts, Moments sharing |
| **Store listing** | Heavily keyword-stuffed ("Pet Vaccine Tracker… Pet Pill Reminder…" repeated). They rely on ASO for discovery. |

**Read:** about 10K+ real downloads after 5 years is **modest traction, not a moat**. They're a capable, responsive indie developer, with almost no presence on iOS or in Portugal.

---

## 2. What users complain about

Themes ranked by how often they appear across both stores. Counts are approximate; one review can hit several themes.

### 2.1 Reminders and recurrence (≈7 reviews), the core feature
- Recurring to-dos: marking one occurrence as done **deleted all future occurrences**.
- A reminder fired **once** and never again. Users want **"nag me daily until it's done"** (flea treatment, deworming).
- **Missing frequencies:** quarterly (deworming), every 2 months, "N times per day/week".
- Upcoming vaccines across several pets were **not sorted by date**, with no alert before the due day (2021, since fixed).
- Setting a recurring deworming reminder was **"not intuitive"** (Dec 2025).
- They patched most of this in 2025, but it ran for years and cost them ratings.

**→ For us:** RRULE-based recurrence with any interval, **reminders that repeat until marked done**, done-without-breaking-the-series, and a single upcoming list sorted by date across all pets. This must be bulletproof in the MVP.

### 2.2 Paywall friction (≈5 reviews)
- "Can only store 3 pets; documents and export are paywalled → uninstalled" (Mar 2026).
- Paying just to use the app properly for 2 cats (Jan 2025, when the free limit seems to have been lower).
- Trying document upload requires paying first. "Stuff locked behind a paywall, and pets are already expensive."
- A "free lifetime" promo turned into a trial-expiry notice for $29/yr, and the user felt tricked.

**→ For us:** our plan already keeps **PDF export free**. Consider a **small free document quota** (e.g. 10 files) so people can try it. Make the paywall honest, with no bait promos.

### 2.3 Reliability and trust (≈25 reviews, mostly from one incident)
- **Dec 2024 promo incident:** AppAdvice featured a "free lifetime Premium today" offer. The purchase flow failed with "Error 16: unknown backend error" and produced **about 20 one-star reviews in one day**. Users called it shady and a scam.
- Save gave **no confirmation**, so users tapped again and created **duplicate pets**, which **couldn't be deleted**.
- **Account deletion failed** (Firestore not-found error). Reinstalling brought the data back.
- The **contact form was broken** and the **support email bounced** (2023), so users couldn't reach support.
- The app hung on intro or login (2024, Dec 2025).
- An update shifted every entered **date by one day** (2022, a timezone bug).
- **Uploading photos from the gallery was broken** for months (2023–24).

**→ For us:**
- Test purchase flows end-to-end (RevenueCat sandbox) before any promo.
- Make saves idempotent, with clear feedback.
- Add **delete and undo** everywhere.
- Test account deletion. Apple requires it, and it's GDPR.
- Use a support channel that works (in-app plus a domain email).
- Store dates as dates, not timestamps.
- A launch spike **amplifies every bug**, so do the beta first.

### 2.4 Data-model gaps (≈10 reviews), where species and life stages don't fit
- **Rescue pets:** no "unknown" option for parents/breed/birthday.
- **Age shown only in years.** Users want days, weeks and months (babies, small animals).
- **Europe:** weight with a **decimal comma (1,79 kg) was rejected**. Users want a **grams** option (cats, small animals, birds).
- **Multi-pet actions:** the same food for several pets had to be entered as an expense per pet.
- **Fosters/rescuers with 30+ animals:** no alphabetical sort, birthday hidden in edit mode.
- **Deceased pets:** a user asked for a memorial mode that freezes the age.
- Requests: multiple profile photos (lookalike pets), vet info (added 2026), logging *past* appointments, dark mode.
- A **quail and chicken keeper** felt the app "might not be for me". Generic profiles don't make non-dog/cat owners feel served.

**→ For us:** this confirms the plan:
- Estimated/unknown dates and fields.
- Locale-aware units (decimal comma, g/kg/lb).
- Multi-pet log and expense entry.
- Pet states (active / rehomed / deceased with memorial).
- Species templates, so a bird or reptile owner feels *seen*.

**New segment to consider:** **fosters and small rescues** (many animals, high turnover, need sorting, bulk actions and handover to adopters). This ties directly to the Phase 4 "pet transfer" idea.

### 2.5 Look and authenticity (2 reviews)
- Praised: "cute", "bubbly", "aesthetic".
- A 1-star review complained about **AI-generated illustrations**: "are there real people behind this?". The user deleted the app on sight.

**→ For us:** a warm, genuine visual identity is a trust signal. Avoid obvious AI art in the brand.

---

## 3. What users praise (the bar to match)
1. **Simplicity.** "Everything I want, nothing I don't." No vet integration needed.
2. **All documents in one place** (vet records, receipts, insurance, licence). Several users picked PetNoter **after trying 2–3 other apps** (PetDesk, VitusVet, mPet), mainly for documents and UI.
3. **No ads, no pop-ups.**
4. **Responsive developer.** Fixes arrived within 24h in some cases, and replies are personal.
5. **Affordable Pro.**
6. Works for **rabbits** and "any pet" at a basic level (notes, weight, expenses, Moments).

Several reviewers said other pet apps failed them before PetNoter. That supports your impression that **the rest of the market is weak**.

---

## 4. Implications for our app

| PetNoter weakness | Our answer | Phase |
|---|---|---|
| Reminders were fragile and inflexible for years | Any-interval recurrence, nag-until-done, one sorted upcoming list | MVP |
| Generic profiles for non-dog/cat species | Species templates with real trackers (shed, feeding, molt, enclosure…) | MVP |
| Export and documents paywalled; 3-pet cap drives uninstalls | Free PDF export, small free doc quota. Test the pet cap in pricing experiments. | MVP / Phase 2 |
| Family sharing Pro-only | Basic household sharing free, sitter links in Premium | Phase 2 |
| No Portuguese, decimal-comma bug, no grams | PT-PT/EN from day one, locale-aware numbers and units | MVP |
| No web app | Expo web | Phase 2 |
| Fosters/rescuers struggle with many animals | Sorting, bulk actions, pet states, transfer to adopter | Phase 2–4 |
| Trust incidents (promo failure, broken support, undeletable data) | Beta before launch, tested purchase and deletion, real support channel | All |
| Little iOS traction | iOS is open territory. Launch on both stores equally. | Phase 2 |

**What NOT to underestimate:** they're active, respond quickly, and already cover the basics with a likeable UI. If our species depth and shared care are only "a bit better", users have no reason to switch. Those features need to be **clearly** better.

---

## 5. What to watch in the survey
- Count mentions of **PetNoter** in `B2` / `D4b` ("Which app(s) do you use?"). If it shows up a lot, it's the main competitor. If not, most people are still on paper, notes or nothing, and the real competitor is **no app at all**.
- Compare app users vs non-users on species mix: are multi-species and exotic owners the ones without a good tool?

---

## 6. Part 2: hands-on teardown checklist (fill in after a week of use)

Create **one dog profile and one reptile or bird profile** to test both paths.

| Area | What to check | Your notes |
|---|---|---|
| Onboarding | Taps and minutes until the first pet is saved. Is sign-up forced? Which sign-in options? | |
| Paywall | When does it first appear? What exactly is free vs Pro? Price in Portugal (monthly / yearly / lifetime)? Trial length? | |
| Add a pet (dog) | Fields offered. Can you mark unknown birthday or breed? Units? | |
| Add a pet (reptile/bird) | Are fields adapted to the species or generic? Anything species-specific at all? | |
| Daily logging | Taps to log "fed" or "gave meds". Is there a quick-log or widget? | |
| Reminders | Create "deworm every 3 months" and "meds twice a day". Mark one done: does the series continue? Does it nag if missed? | |
| Vaccines | Add past and upcoming vaccines. Is there a sorted upcoming list across pets? | |
| Weight | Enter 1,79 kg and 45 g. Is there a chart? | |
| Documents | Upload a PDF and a photo (Pro?). Can it read the content (AI)? | |
| PDF export | What's in the report? Would a vet find it useful? | |
| Sharing | How does family sharing work? Can a sitter get temporary access? | |
| Expenses | Per pet only, or split across pets? | |
| AI assistant | What does it answer? Does it use the pet's records? Disclaimers? | |
| Notifications | Do they arrive on time? Are they annoying? | |
| Language | Is a Portuguese UI available on Android? How is the quality? | |
| Account | Can you export your data? Delete the account? | |
| Overall feel | 3 things you'd copy, 3 things you'd do differently | |

---

### Sources
- Google Play listing and all written reviews for `com.petnoter` (accessed 19 Nov 2026)
- App Store listing and customer reviews (US, GB, IE, FR, ES, NL, IT, IN) for id `1580581463` (accessed 19 Nov 2026)
- [petnoter.com](https://petnoter.com) (features and free vs Pro table)
