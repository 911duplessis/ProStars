# PROSTARS™ — Mobile App Roadmap (Phase 3)

> Companion to `PROSTARS_MASTER_PLAN.md` §5–6 and `WEBSITE_BLUEPRINT.md`. **Trigger condition, not a date:** this phase starts when there are enough coaches and classes running that parents are checking the website dashboard on their phones weekly and asking for something faster — not before. Building an app for a single-coach, single-location business is the clearest way to spend money that should be going toward a second coach.

---

## Why an app at all (and why not yet)
The website dashboard (Phase 2) already does the job of surfacing progress and handling bookings. An app is justified only once push notifications, offline access, and habitual daily/weekly use start to matter — i.e. once PROSTARS has enough recurring family volume that a home-screen icon changes retention. Until then, a mobile-responsive website *is* the mobile experience.

---

## Two Experiences

### Child Experience (the "Star" side)
Design constraint: **never a shame mechanic.** No public leaderboards, no losing streaks, no comparison between children. Every mechanic below rewards a child's *own* trajectory only.

- **Challenges:** short, coach-assigned or self-directed micro-challenges tied to the Method's outer layer (e.g. "try this three times even if you don't get it" → Trying/Achievement).
- **Badges:** earned per Method trait, illustrated by the rotating "Star Guide" character cast (see `BRAND_IDENTITY.md`), never taken away or reset.
- **Achievements:** milestone certificates surface here first, shareable to a parent's phone.
- **Games:** light, short-session mini-games reinforcing regulation/observation skills (e.g. a breathing-paced game for Presence, a simple pattern-matching game for Observation) — designed with a child-development lens, not addictive-engagement design patterns. No infinite scroll, no session length optimised for retention over wellbeing.
- **Learning activities:** simple at-home extensions of the week's inner-layer focus, for parents to do with their child for five minutes — this is what turns a once-a-week class into a daily-life habit, and it's the single highest-leverage feature for actually delivering on "becoming more okay, week by week."

### Parent Experience (the "Family" side)
- **Progress:** same feed as the Phase 2 web dashboard, native and push-notified.
- **Communication:** coach messages, term announcements, event reminders — push, not email, once this exists.
- **Bookings:** term re-enrolment, holiday programme sign-up, birthday package enquiries, from a phone in under a minute.
- **Payments:** stored payment method, auto-charge for term renewal (opt-in), receipts.

---

## Technical Approach
- **React Native (Expo)**, sharing the same Supabase backend, auth, and data model built for the Phase 2 website — this is a client on top of existing infrastructure, not a rebuild.
- **Expo push notifications** for coach messages and reminders.
- Single codebase for iOS and Android; no native Swift/Kotlin investment at this stage.
- Ship parent-side first (higher-value, lower-risk), child-side second once the badge/challenge content library exists.

## What NOT to build in Phase 3
- No social feed between families (privacy risk, and off-brand — this is not a social network).
- No in-app chat between children.
- No advertising, no third-party data sharing, no "invite a friend" viral mechanics aimed at kids.
- No franchise/multi-location admin tooling — that belongs in a future Phase 4 franchise operations platform, scoped only once `FRANCHISE_MODEL.md`'s licensing structure is actually operating with more than one coach.
