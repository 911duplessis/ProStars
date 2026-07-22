# PROSTARS™ — Website Blueprint (Phase 2)

> Companion to `PROSTARS_MASTER_PLAN.md` §5–6. This is the spec for the Phase 2 digital platform — built only once Phase 1 (in-person classes, manual booking, WhatsApp community) has real paying, retained families. Building this earlier is the single most common way ventures like this burn Year 1 on software instead of children.

---

## Design principle
The website's job is to do two things Elanza cannot scale by hand once she has more than ~30 families: **take bookings without a phone call**, and **make the outer-layer proof (Achievement / Respect / Strength) visible to a parent without her having to type it out every week.** Everything else is optional polish.

---

## Sitemap

```
/                       Home — mission, Method summary, "book a trial class" CTA
/method                 The PROSTARS METHOD explained (inner + outer layer, plain language)
/classes                Class finder: age band, day, location, price, availability
/classes/[slug]         Single class page — booking widget embedded
/holiday-programmes     Seasonal programme listing + booking
/birthday-experiences   Birthday party package page + enquiry form
/for-schools            School partnership pitch (see FRANCHISE_MODEL.md for pricing logic)
/about                  Founder story (Elanza), coach bios once there are more than one
/gallery                Photo/video gallery, opt-in per family (see privacy note below)
/blog                   Optional — parent-facing content on the Method's inner-layer traits
/contact
/login                  Parent portal auth
/dashboard              Parent dashboard (auth-gated) — see below
```

---

## Feature Spec

### 1. Booking
- Term-based enrolment (align to NZ school terms) and single-session/casual booking for holiday programmes.
- Real-time capacity per class (12–15 child cap, per `PROSTARS_MASTER_PLAN.md` §5) — sold out classes show a waitlist join, not a broken form.
- Sibling discount logic built in from day one (Elanza's own household is two kids — this will matter to her customer base too).
- Payment via Stripe (card) with the option to fall back to bank transfer for term fees, mirroring how EM Muller Services already handles NZ customer payments.

### 2. Parent Dashboard
- Which PROSTARS METHOD inner-layer skill(s) their child's class is focused on this term, in plain language (not clinical).
- Outer-layer proof feed: short coach notes, tagged Achievement / Respect / Strength, timestamped per session — this is the digitised version of the Phase 1 WhatsApp update.
- Digital certificates (downloadable/shareable PDF or image) triggered at milestones.
- Upcoming session reminders and term calendar.
- Direct, moderated message thread to the child's coach (not open chat — asynchronous, coach-reviewed).

### 3. Child Progress Tracking
- Per-child record, visible only to that child's parent(s) and the coach — never a public leaderboard, never a cross-child comparison.
- Structured around the Method's outer layer only (Achievement / Respect / Strength); inner-layer data (Presence, Resilience, Observation, Social, Trying) stays qualitative and coach-facing, not scored, to avoid ever drifting into diagnostic/clinical territory this brand explicitly avoids (see `BRAND_IDENTITY.md`).

### 4. Certificates, Photos, Newsletters, Events
- Auto-generated milestone certificates (on-brand template, not generic clip-art).
- Family-level photo/video opt-in — default off, explicit per-family consent, easy to revoke (this matters more than usual given the source material for this brand includes a family WhatsApp media archive; PROSTARS' own customer data practices should be visibly stricter and clearer than that).
- Termly newsletter (email) summarising the term's Method focus and upcoming holiday programmes.
- Event listing for open days, school holiday programmes, and community sessions.

---

## Technical Architecture

| Layer | Choice | Why |
|---|---|---|
| Frontend/framework | **Next.js** (App Router) | Fast to ship, strong SEO for local search ("kids activities Christchurch"), one codebase for marketing site + authenticated dashboard |
| Hosting | **Vercel** | Zero-ops deploys, generous free/starter tier appropriate to single-location scale, trivial preview deployments for iterating on copy/design with Elanza |
| Backend/data | **Supabase** | Postgres database, built-in auth (parent accounts), row-level security (critical for keeping one family's child data invisible to another family), file storage (photos/certificates) — all in one platform, no separate backend to run |
| Payments | **Stripe** | NZD support, Stripe Checkout for term fees, minimal PCI burden |
| Email | Supabase + a transactional provider (e.g. Resend) | Newsletters, booking confirmations, reminders |

### Data model (minimum viable)
```
families        (parent auth identity, contact info, consent flags)
children        (name, DOB/age band, family_id)
classes         (name, age_band, location, day/time, capacity, price)
enrolments      (child_id, class_id, term, payment_status)
sessions        (class_id, date)
progress_notes  (child_id, session_id, method_trait, note_text, photo_url?)
certificates    (child_id, milestone_type, issued_date, file_url)
```

### MVP scope discipline
Ship in this order, and stop between each until it's actually needed:
1. Static marketing site + `/method` + embedded external booking form (no custom backend at all).
2. Real booking + payments (Stripe Checkout, no dashboard yet).
3. Parent dashboard read-only (progress feed, certificates).
4. Coach-facing note entry tool (replaces the Phase 1 WhatsApp habit).
5. Everything else in this document, only once 1–4 are proven with real families.

**Do not build in Phase 2:** a native app (that's Phase 3, see `APP_ROADMAP.md`), multi-location/franchise admin tooling (that's Phase 4+, see `FRANCHISE_MODEL.md`), or any scored/gamified public leaderboard.
