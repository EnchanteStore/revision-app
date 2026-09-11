# Google AI Studio handoff — Aster Revision

## How to import this project

Use Google AI Studio **Build mode** and choose **Import from GitHub** from the Add files menu. Import this repository as an existing full-stack project.

Do **not** choose AI Studio's native Android project mode for this repository. That mode generates Kotlin and Jetpack Compose, while the existing mobile application is a shared Expo/React Native application for both iOS and Android.

Before importing:

1. Commit only the intended project changes to a private GitHub repository.
2. Exclude `.env`, credentials, source materials, licence evidence, patient data and production database exports.
3. Keep the repository private. The platform contains commercial product logic and will later contain licensed educational workflows.
4. Confirm that the latest Supabase migrations have been backed up and reviewed before applying them.

## Ready-to-paste continuation prompt

```text
Continue development of this existing Aster Revision repository. Do not regenerate it or replace its architecture.

Product:
- Independent UK pharmacy registration-assessment revision platform.
- Not affiliated with, endorsed by or approved by the GPhC or PSNI.
- Educational revision only, not clinical advice, and no pass guarantee.
- Never use leaked, recalled, reconstructed, paid-bank or official examination questions.
- AI-generated educational content must remain draft-only until pharmacist review and a separate publication action.

Architecture to preserve:
- Existing Next.js/Vinext web application at the repository root.
- Existing Expo Router mobile application at apps/mobile for both iOS and Android.
- Permanent bundle/package identifier: uk.co.asterrevision.app.
- Shared domain contracts in packages/domain.
- Supabase Auth/PostgreSQL/Storage/RLS as the identity, content and entitlement authority.
- Stripe is web-only. Native mobile purchasing uses Apple/Google billing and writes to the same entitlement ledger only after server verification.
- Do not move secrets or service-role credentials into browser or mobile code.
- Do not replace Expo with Kotlin/Jetpack Compose, Flutter or a second mobile codebase.

Authorization invariants:
- Students receive only published, entitled content.
- Approved but unpublished content remains hidden.
- Timers, question selection, scoring, answer release and entitlement decisions remain server-authoritative.
- Students never receive source files, source chunks, AI raw output, prompts, reviewer notes or premature answer keys.
- Staff privileges are database-backed and cannot be assigned from client metadata.
- Reviewer approval requires the existing verified-pharmacist and AAL2 checks.
- Subscription non-payment grace is exactly 24 hours after the paid-through period ends.

Mobile state:
- Expo SDK 57 workspace is implemented under apps/mobile.
- Email/password, Apple and Google sign-in foundations are present.
- First-time social users must complete the server-controlled age-band onboarding flow.
- Dashboard, plan, learning, questions, timed quizzes, mocks, results, flashcards, analytics, reporting and account screens are present.
- Questions and mocks use protected-screen capture deterrence, masked watermarking and forensic telemetry.
- The app is online-only and must not cache clinical content for offline access.
- Native purchases are intentionally disabled until store configuration and server verification are complete.

Current verification evidence:
- npm run mobile:typecheck passes.
- ESLint for apps/mobile passes.
- Mobile unit tests pass: 3/3.
- Repository schema/security assertions pass: 24/24.
- Expo dependency compatibility check passes against the installed SDK map.
- Android and iOS Expo production bundle exports pass.

Known release blockers:
- The Supabase mobile migration has not been confirmed as applied to production.
- Apple, Google, EAS, signing and store-product configuration are not present.
- MOBILE_IAP_VERIFIER_URL and MOBILE_IAP_VERIFIER_TOKEN require a real Apple/Google verification service.
- TestFlight and Play internal builds have not been created or distributed.
- npm audit currently reports production dependency advisories, including a critical Next.js advisory. Review upgrades without breaking Expo/Vinext compatibility.
- The overall root web type-check contains pre-existing errors in a duplicate app/page 2.tsx file and the web mock-results page. Do not delete user files without inspecting ownership and intent.

Working rules:
1. Inspect package.json, apps/mobile/README.md, README.md, the latest Supabase migrations and tests/schema-design.test.mjs before editing.
2. Run git status first. Preserve unrelated and user-owned changes.
3. Propose a short implementation plan before modifying files.
4. Make additive migrations; never rewrite applied migrations in a production-connected environment.
5. Keep native purchasing feature-flagged off until real store verification is tested.
6. Never claim a migration, signed build, payment flow or deployment passed unless you actually ran and recorded it.
7. After changes, run the narrowest relevant tests, mobile type-check, mobile lint, Expo dependency checks and platform bundle exports.

First task:
Audit the imported repository against this handoff. Report any missing files, configuration gaps and contradictions. Do not change architecture or deploy anything until that audit is complete.
```

## Important files

- `apps/mobile/README.md` — mobile setup, native billing gates and security boundaries.
- `apps/mobile/app.config.ts` — Expo application identifiers and native plugins.
- `apps/mobile/src/providers/session-provider.tsx` — authentication and required account gates.
- `apps/mobile/src/providers/billing-provider.tsx` — direct native purchase client foundation.
- `apps/mobile/src/components/protected-content.tsx` — protected-screen and watermark behavior.
- `packages/domain/mobile.ts` — shared mobile entitlement and disclaimer contracts.
- `lib/mobile-purchases.ts` — server-side verification adapter and entitlement reconciliation.
- `supabase/migrations/202609110017_expo_mobile_foundation.sql` — native product mappings, purchase records and mobile session controls.
- `tests/schema-design.test.mjs` — static authorization and architecture assertions.

## Secrets to configure outside source control

Mobile public build variables:

- `EXPO_PUBLIC_SUPABASE_URL`
- `EXPO_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- `EXPO_PUBLIC_API_BASE_URL`
- `EXPO_PUBLIC_GOOGLE_WEB_CLIENT_ID`
- `EXPO_PUBLIC_GOOGLE_IOS_CLIENT_ID`
- `EXPO_PUBLIC_EAS_PROJECT_ID`
- `EXPO_PUBLIC_MOBILE_IAP_ENABLED=false` until release gates pass

Server-only variables:

- Supabase server credential
- Stripe server and webhook credentials
- `MOBILE_IAP_VERIFIER_URL`
- `MOBILE_IAP_VERIFIER_TOKEN`
- AI-provider API credentials

Do not paste secret values into AI Studio chat or commit them to GitHub.

## Platform limitation

AI Studio can help edit the imported TypeScript repository, but Expo development-client generation, iOS signing, TestFlight submission and physical-device verification still need Expo/EAS, Xcode/App Store Connect and Google Play Console outside AI Studio. Treat AI Studio previews as development evidence, not store-release evidence.
