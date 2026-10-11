# Policy integration audit — October 10, 2026

Scope: published Google Play catalog plus the studio's named active development games. Read-only source audit; only website policies were changed. Runtime initialization, service implementations, flags and platform manifests were inspected separately from package dependencies.

## Evidence limits

This audit establishes the inspected local implementations, not an independently verified mapping to installed store binaries, deployment environment variables, live Firebase retention settings or store-console Data Safety/App Privacy answers. Do not treat a local dependency, simulated purchase, policy publication, store upload or pushed branch as proof that a feature shipped. Several game checkouts contain uncommitted work. No game repository was changed by this task.

## Inspected checkouts

| Project | Branch / commit at audit | Local tree | Main evidence |
| --- | --- | --- | --- |
| BlackJackRoguelike (Neon 21) | master / 3271e81 | Untracked/local changes | lib/main.dart:70–111; services under lib/services; lib/core/constants/monetization_constants.dart |
| CathedralCollapse | main / 19fb6d7 | Clean | lib/app/bootstrap.dart:18–46; lib/main.dart:12–37; lib/app/onboarding/country_resolver.dart:26–82; lib/app/builder/qr_scan_screen.dart |
| CascadeBlast | master / 0fdf91b | Untracked outputs | lib/main.dart; lib/services/analytics; ads_service.dart; iap_service.dart; age_assurance_service.dart |
| ColorMerge | master / 8b443a4 | Clean | lib/main.dart; lib/core/services/ad_service.dart and monetization_service.dart; lib/core/analytics/providers |
| CaseBook | main / ae56c95 | Uncommitted changes | lib/main.dart:26–27; lib/firebase/firebase_bootstrap.dart; firebase_remote_config_service.dart; lib/monetization/billing_service.dart:38–87 |
| Veilborn | master / 823e793 | Uncommitted changes | lib/services/firebase_telemetry.dart; rewarded_ads_service.dart; age_signals_service.dart; store_iap_service.dart; age_access_host.dart; ios/Runner/AppDelegate.swift |
| Witchmark | master / 60e138c | Uncommitted changes | pubspec.yaml; lib/main.dart:34–53; lib/data/repository.dart; lib/services/iap_service.dart; gacha_service.dart |
| Embertable (Flutter) | Separate Documents/ChatGPT/EmberTable checkout | Audited read-only | lib/main.dart:36–40; lib/engine/commerce.dart; receipt_verifier.dart; native_commerce.dart; rewarded_ads.dart; services receipt-verifier documentation |
| ShipRekt | codex/codebase-quality-pass / 870c045 | Uncommitted changes | app/pubspec.yaml; app/lib/main.dart; app/lib/game/progress.dart |
| Human Resources | codex/hr-game-rewrite / 25db3fd | Uncommitted changes | app/pubspec.yaml; app/lib/main.dart; app/lib/induction/induction_screen.dart |
| Short Circuit | shortcircuit main / d789bb7; short-circuit main / ef46f5e | First clean; second uncommitted | Both pubspec.yaml/main.dart; older Hive/font path and newer SharedPreferences implementation |

EmberTable-Godot is a separate unnamed development game, not the Flutter Embertable covered by the named policy. Its current implementation is local-only; no named public listing was invented for it.

## Findings reflected in published policy

- Neon 21: active Firebase Analytics/Crashlytics, Android Performance plugin, Amplitude event analytics, Android TikTok attribution including purchase conversion, rewarded AdMob, UMP, opt-in local notifications, transient store age checks. Real store purchases; no custom verifier found. Runtime Google Fonts fetching disabled. No visual replay.
- Cathedral Collapse: active Firebase Analytics/Crashlytics/Performance, Amplitude, ads and local notifications. TikTok depends on build configuration. Country resolution sends a network request to ipapi.co alongside SIM information; camera QR decoding is local. No active IAP implementation despite its dependency. No replay.
- Cascade Blast: Analytics, release Crashlytics, rewarded/interstitial AdMob, native reminders, store billing/restores, coarse Play age checks. Remove Ads leaves rewarded ads available. Technical age-check failures can fail open; do not promise universal verified-age protection.
- Color Merge: Analytics, rewarded/interstitial AdMob and store billing. TikTok provider and reminder service are stubs. No Crashlytics/Amplitude/replay. Remove Ads leaves rewarded ads available. Google Fonts can download at runtime.
- The Casebook: Analytics, release Crashlytics, Remote Config and rewarded AdMob. Default DevBillingService grants packs locally with no store charge/real restore. New local age-assurance work is not invoked by startup. Google Fonts can download at runtime. Do not equate current development billing with the production store binary.
- Veilborn: telemetry initially disabled; age-confirmed adult and iOS tracking authorization gate ads/Analytics/Crashlytics. Platform age and approval signals are transient; configured significant-update Apple approval records may use iCloud key-value storage. ATT permission is now requested, contradicting old policy. Android AD_ID permissions are removed. Rewarded placements include offline XP, daily egg and quest rewards. Native store handling is default; optional HTTPS verifier depends on build configuration.
- Witchmark: current prototype uses local Hive data; no active ad/analytics/crash/attribution SDKs. Real IAP disabled and real implementation unimplemented; Moonstone/Reading flows simulated. Intended audience explicitly confirmed by user as teens/adults 13+, not 18+. Store rating is not established by this decision.
- Flutter Embertable: AdMob/UMP, local saves, no analytics/crash/replay. Store products and configured HTTPS receipt verifier required for real billing. No verifier endpoint bundled. Receipts can be stored locally pending verification.
- ShipRekt, Human Resources and Short Circuit: audited development paths store progress locally and have no active ads/analytics/crash/billing integrations. Older Short Circuit version uses Google Fonts; newer rebuild does not.

## Follow-up outside this website change

- No UMP consent implementation found in the inspected Cathedral Collapse, Cascade Blast, Color Merge or Casebook runtime paths. A policy or Terms acceptance is not SDK consent. Review regional SDK behavior, consent requirements and store declarations before releases; do not claim these controls were added by this task.
- Confirm each production binary/build flag and store-console disclosure against its actual enabled integrations, especially conditional TikTok, simulated-vs-real billing, age handling and the Veilborn optional verifier.
- Store ratings and region-specific purchase restrictions are separate from Witchmark's intended audience. Changing website wording does not update store ratings or implement new age controls.
- Firebase property retention, ad partner configuration and provider deletion operations were not inspected.

## Provider references consulted

- https://firebase.google.com/support/privacy
- https://developers.google.com/admob/flutter/privacy
- https://support.google.com/googleplay/android-developer/answer/10144311
- https://ipapi.co/privacy/
- https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement

## Verification

Policies reviewed against source findings by the agents that audited each group. Check local links/fragments, HTML structure, old blanket statements, responsive layout, deployment commit and live served content before completion.
