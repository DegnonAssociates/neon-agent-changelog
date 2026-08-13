---
title: Changelog
---

# Changelog

This file tracks the app's development history in reverse chronological order.
Entries are based on the repository's commit history and group related features,
updates, and bugfixes by the date they landed.

## 2026-08-10 - Event Attendee Exports and Model Configuration

### Features

- Added the `event_attendee_export` deterministic intent for requests such as
  "Export attendees for the 2024 Annual Meeting."
- Extended report generation so confirmed event attendee exports can include
  attendee details, registration details, ticket/package information, matched
  user records, and account custom field data.
- Added seeded evaluation prompts for attendee export requests.

### Updates

- Changed OpenAI configuration so the chat path requires an explicit model
  setting instead of silently relying on an implicit default.
- Hardened the OpenAI client setup and chat error path around missing model
  configuration.

## 2026-07-30 - Company History Lookup

### Features

- Added a deterministic company/employer history intent for questions about a
  person's company name, employer, current role, and employment history.
- Added company lookup rules to the agent configuration, including fallback from
  `company_contact.companyName` to `company.name` through `companyId`.
- Improved prompt submission handling in the chat client so follow-up prompts
  are handled more reliably.

## 2026-07-21 - Tenant-Aware Microsoft Entra Authentication

### Features

- Protected the app and API routes with Auth.js and Microsoft Entra ID OIDC.
- Added a dedicated login page and session-aware application shell behavior.
- Added tenant claim validation so only users from the configured Entra tenant
  can sign in.
- Added host-derived callback URL support with `trustHost`, allowing localhost
  and deployed hosts to use the same callback path without hard-coded `AUTH_URL`
  or `NEXTAUTH_URL` values.
- Added NextAuth type augmentation for app-specific session fields.

### Updates

- Documented required Microsoft Entra environment variables and redirect URI
  setup in `README.md`.
- Added an authentication guard helper and applied it to chat, assistant review,
  assistant report, and report download endpoints.
- Updated assistant observability and report scripts so review/report data can
  be associated with authenticated users.
- Added `next-auth` as an application dependency.

### Bugfixes

- Fixed deployed-host auth callback behavior by deriving callback URLs from the
  current request host.
- Removed the previous version bump workflow during the auth integration to
  avoid conflicting release automation behavior.

## 2026-06-29 - Assistant Review Logging and Field Clarifications

### Features

- Improved assistant review logging with richer request and user context.
- Added evaluation coverage for user field and account custom field lookup
  prompts.

### Updates

- Clarified deterministic user field responses so user-table fields and account
  custom fields are distinguished more consistently.
- Updated the chat client review controls and review API payload handling.
- Bumped the app version to `0.1.10`.

## 2026-06-01 - Client Configuration Expansion

### Updates

- Added additional client definitions to the Neon client data configuration.
- Prepared the multi-client data source map for more organizations using the
  `{CLIENT}_{DATASOURCE}_URL` environment-variable convention.

## 2026-05-21 - Multi-Client Neon Support and Custom Field Intents

### Features

- Added multi-client support so the chat UI sends a selected `clientId` and the
  server scopes database work to that client.
- Added `lib/clientData.ts` for client-specific Neon connection configuration.
- Expanded database helpers to support client-specific archive queries.
- Added deterministic account custom field lookup support for prompts such as
  "What is Mark Palmert's YearCertified?"
- Added custom-field matching rules for canonical user fields, account custom
  data, field dictionary labels, and option-backed values.

### Updates

- Updated the app layout and chat client to expose client selection.
- Documented client data sources and business rule configuration in `README.md`.
- Expanded business rules and deterministic intent configuration for custom
  field behavior.
- Bumped the app version through `0.1.8` as release automation ran across the
  merged work.

## 2026-04-23 - Fallback Context and Version Automation Fixes

### Features

- Added fallback insight generation to provide more helpful context when an
  answer cannot be resolved through deterministic paths.
- Added `lib/fallbackInsights.ts` to centralize fallback analysis and suggested
  next-step behavior.

### Updates

- Added and then corrected automatic PR version bump workflow behavior.
- Bumped the app version through `0.1.5`.

### Bugfixes

- Fixed the version bump workflow so automated release commits can avoid
  retriggering themselves with skip markers.

## 2026-04-22 - Report Exports and Deterministic Lookup Refinements

### Features

- Added downloadable Excel report exports.
- Added `app/api/reports/[reportId]/route.ts` to serve generated report files.
- Added `lib/reportArtifacts.ts`, `lib/userReportIntent.ts`, and
  `scripts/generate_report_workbook.py` for report artifact creation.
- Expanded deterministic user meeting and session history lookups, including
  count-style questions.
- Expanded person lookup and attendance fallback behavior for ambiguous or
  missing records.
- Added a deployment workflow and automatic PR version bump workflow during
  release process iteration.

### Updates

- Refined member, meeting, attendance, and user-history question handling.
- Expanded seed evaluation prompts for membership, meeting, session, and user
  history coverage.
- Bumped the app version through `0.1.3`.

### Bugfixes

- Improved deterministic handling for ambiguous person matches and attendee
  records where direct user IDs are missing.
- Added temporary workflow validation commits and later removed the temporary
  test artifact.

## 2026-04-21 - Membership, Meeting, and User Overview Intents

### Features

- Added deterministic membership and meeting quick paths.
- Added a deterministic `user_overview` intent for prompts such as "Tell me
  about Paul Baer."
- Expanded structured user summaries to include memberships, registrations,
  attendance, sessions, donations, and custom fields when available.

### Updates

- Added seed evaluation prompts for user overview and membership/meeting quick
  path behavior.
- Bumped the alpha release version to `0.1.1`.

## 2026-04-20 - Assistant Observability and Review Reports

### Features

- Added PostgreSQL-backed assistant observability with run logging in
  `assistant_observability.assistant_run`.
- Added assistant review capture in `assistant_observability.assistant_review`.
- Added review buttons below assistant answers in the chat UI.
- Added `/api/assistant-report` for JSON performance reports.
- Added `/api/assistant-review` for staff review labels.
- Added `npm run assistant-report` and `scripts/assistant-report.mjs` for
  terminal summaries.
- Added `config/business_rules.json` and `evals/seed_eval_cases.json` to track
  business rules and evaluation prompts.

### Updates

- Documented the assistant review workflow and `OBSERVABILITY_DB_URL`
  configuration in `README.md`.
- Expanded assistant telemetry to track repeated questions, fallback-heavy
  prompts, high-token prompts, deterministic intent usage, promotion candidates,
  and reviewed correct answers suitable for eval cases.

## 2026-04-17 - Deterministic Intent Refactor

### Features

- Extracted deterministic intent matching and answer generation into
  `lib/deterministicIntents.ts`.
- Added `config/deterministic_intents.json` to configure deterministic quick
  paths outside the main chat implementation.
- Added `lib/chatTypes.ts` for shared chat response typing.

### Updates

- Reduced the amount of deterministic intent logic embedded directly in
  `lib/amsChat.ts`.
- Styled the chat scrollbar for a more polished chat experience.

## 2026-04-16 - Chat Polish and Archive Query Hardening

### Features

- Added a fixed chat composer to keep prompt entry available while reviewing
  longer conversations.
- Added direct member history lookup support.

### Updates

- Reworked the chat UI with a more complete assistant experience.
- Expanded AMS agent configuration and SQL guidance for archive-safe answers.
- Added stronger Postgres typing support with `@types/pg`.

### Bugfixes

- Hardened archive query handling to keep the assistant on read-only,
  database-grounded answers.
- Improved query and answer handling around archived membership and attendance
  data.

## 2025-12-02 - Project Creation and Initial AMS Assistant Boilerplate

### Features

- Created the app from the Next.js starter template.
- Added the first AMS chat API route.
- Added OpenAI and PostgreSQL dependencies.
- Added initial database, OpenAI client, SQL tool, AMS chat, and agent
  configuration modules.
- Moved shared library code from `app/lib` to the root `lib` directory to match
  the project structure used by later development.

### Updates

- Established the baseline Next.js, React, TypeScript, Tailwind, ESLint, and
  package scripts used by the app.
