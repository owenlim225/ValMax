# Valmax MVP Implementation Brief

## 1. Readiness Verdict

MVP is ready for implementation planning, with two explicit assumptions:

- Production payments use XLM on Stellar Mainnet, while development and QA use Stellar Testnet.
- MVP uses wallet-based identity, with no email/password account requirement.

Implementation should start with a narrow vertical slice: one admin-created event, booth directory, wallet connect, paid strategy unlock, and offline checklist.

## 2. Recommended Stack

Frontend:
- Next.js App Router.
- TypeScript.
- Tailwind CSS or a minimal component system.
- PWA service worker for offline checklist caching.

Backend:
- Next.js route handlers or server actions for app API.
- Supabase Postgres for data.
- Supabase Storage or S3-compatible storage for strategy images.
- Supabase Row Level Security for row ownership and browser-safe access controls.

Blockchain:
- Stellar XLM payment operation.
- Freighter wallet integration for MVP wallet connect/signing.
- `@stellar/stellar-sdk` for transaction building and server-side verification support.
- Horizon or Stellar RPC for transaction lookup, depending on final implementation preference.

Why this stack:
- Next.js App Router is a current React app architecture with file-system routing and server/client component support.
- Supabase gives fast Postgres-backed CRUD, auth-compatible policies, storage, and admin workflows without building infrastructure from scratch.
- Stellar docs provide JavaScript payment app examples and wallet integration guidance.
- Service workers are the standard web primitive for offline-capable app behavior.

## 3. Evidence Notes

- Stellar's JavaScript payment tutorial uses `@stellar/stellar-sdk` and notes wallet integration packages including Freighter-related packages.
- Stellar Freighter docs describe Freighter as a browser extension wallet and provide React dapp integration and signing flows.
- Stellar payment docs show native XLM payment operations and transaction confirmation patterns.
- Stellar Horizon provides API reference for reading Stellar network data.
- MDN states service workers sit between app, browser, and network and enable offline experiences.
- Supabase states RLS should be enabled for tables in exposed schemas and can combine with Auth for end-to-end security.
- Next.js App Router is documented as a file-system router using React Server Components, Suspense, and Server Functions.

Source URLs:
- https://developers.stellar.org/docs/build/apps/example-application-tutorial/overview
- https://developers.stellar.org/docs/build/guides/freighter
- https://developers.stellar.org/docs/build/guides/freighter/integrate-freighter-react
- https://developers.stellar.org/docs/build/guides/freighter/prompt-to-sign-tx
- https://developers.stellar.org/docs/build/guides/transactions/send-and-receive-payments
- https://developers.stellar.org/docs/data/apis/horizon/api-reference
- https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- https://supabase.com/docs/guides/database/postgres/row-level-security
- https://nextjs.org/docs/app

## 4. System Architecture

Client app:
- Renders event, strategy, booth, and checklist UI.
- Connects wallet.
- Requests payment intent from backend.
- Sends signed transaction/hash to backend.
- Saves started strategy snapshots and checklist progress locally.
- Queues offline checklist updates.

App API:
- Serves event, booth, strategy, comment, and reaction data.
- Creates payment intents.
- Verifies Stellar payment transaction details.
- Grants strategy unlocks.
- Enforces moderation and ownership rules.
- Provides admin endpoints.

Database:
- Stores event data, booth data, strategies, strategy steps, comments, reactions, reports, unlocks, and checklist progress.
- Uses policies so regular users can read public event data but cannot mutate admin or other-user data.

Stellar network:
- Handles XLM payment transaction.
- Source of truth for confirmed payment existence.
- App stores transaction hash and verified amount/destination/memo.

Offline storage:
- IndexedDB preferred for cached checklist snapshot and step state.
- Service worker caches shell and required assets.
- Local queue syncs progress when online.

## 5. Payment Flow

1. User opens paid strategy.
2. User connects Stellar wallet.
3. User taps Unlock.
4. Backend creates payment intent:
   - strategy id
   - buyer id/public key
   - expected amount
   - destination account
   - memo or unique reference
   - expiry
5. Client builds or receives unsigned payment transaction.
6. Wallet prompts user to sign.
7. Client submits signed transaction or returns signed XDR/hash depending on implementation.
8. Backend verifies:
   - transaction exists and is successful
   - destination matches Valmax receiving account
   - amount matches expected XLM
   - memo/reference matches payment intent
   - source account matches buyer public key or accepted wallet account
   - transaction has not already been used for another unlock
9. Backend marks unlock confirmed.
10. Client receives access and can start/download checklist.

MVP simplification:
- Use one platform receiving wallet.
- Track strategist share internally.
- Admin handles payout settlement after MVP validation.

## 6. Data Model

Tables:
- users
- events
- booths
- swag_offers
- line_reports
- strategies
- strategy_steps
- strategy_images
- unlocks
- checklists
- checklist_steps
- comments
- reactions
- reports
- admin_actions
- analytics_events

Important indexes:
- strategies(event_id, status, visibility)
- strategy_steps(strategy_id, position)
- booths(event_id)
- comments(target_type, target_id, created_at)
- reactions(target_type, target_id, author_id)
- unlocks(strategy_id, buyer_id)
- unlocks(stellar_tx_hash)
- line_reports(booth_id, created_at)

## 7. Authorization Rules

Public read:
- Live event metadata.
- Published booth and sponsor data.
- Public swag offer summaries.
- Published free strategies.
- Paid strategy previews.

Authenticated wallet read:
- Own unlocks.
- Own started checklists.
- Full paid strategy only after confirmed unlock.

Authenticated wallet write:
- Own strategies.
- Own comments.
- Own reactions.
- Own line reports.
- Own checklist progress.

Admin write:
- Events.
- Booths.
- Moderation status.
- Admin verification flags.

Server-only:
- Payment confirmation.
- Unlock status confirmation.
- Platform fee records.
- Strategist payout records.

## 8. Phased Build Plan

Phase 0: Setup
- Create Next.js app with TypeScript.
- Configure Supabase.
- Add database schema and RLS.
- Add seed data for one sample Web3 event.

Phase 1: Public event intelligence
- Event home.
- Booth directory.
- Strategy browse.
- Strategy detail previews.

Phase 2: Wallet and payment proof
- Wallet connect.
- Payment intent API.
- XLM payment signing and submission.
- Payment verification.
- Unlock gate.

Phase 3: Strategy creation and feedback
- Strategy builder.
- Image upload.
- Comments.
- Likes/dislikes.
- Report flow.

Phase 4: Offline checklist
- Start strategy.
- Cache checklist snapshot.
- Offline checklist view.
- Local progress persistence.
- Sync when online.

Phase 5: Admin and moderation
- Event/booth admin CRUD.
- Content moderation.
- Revenue/unlock inspection.
- Basic analytics.

## 9. Test And Verification Expectations

Unit tests:
- Fee calculation.
- Payment intent expiry.
- Unlock access rules.
- Strategy visibility rules.
- Checklist progress merge.
- Reaction uniqueness.

Integration tests:
- Guest cannot view locked paid strategy body.
- User can unlock after verified payment.
- Invalid transaction does not unlock.
- Duplicate transaction hash cannot unlock two strategies.
- User cannot edit another user's content.
- Admin can hide content.

End-to-end tests:
- Browse event as guest.
- Connect wallet mock.
- Unlock paid strategy with mocked confirmed transaction.
- Start strategy and check off steps.
- Simulate offline mode and refresh checklist.
- Reconnect and sync checklist progress.
- Create strategy and publish.
- Comment and react.
- Report strategy and hide as admin.

Manual QA:
- Mobile 360px, 390px, 430px widths.
- Poor network throttling.
- Offline after checklist start.
- Payment timeout and failure.
- Long strategy title and long booth names.

## 10. MVP Acceptance Criteria

Build is MVP-ready when:
- Admin can create one event and add at least 20 booths.
- Users can browse event and booth data without wallet connection.
- Users can connect a Stellar wallet.
- Users can unlock one paid strategy by paying XLM.
- Unlock is granted only after server-side transaction verification.
- Users can start an unlocked strategy.
- Checklist is available offline after start.
- Users can check off steps offline and sync later.
- Users can create a free or paid strategy with text, images, and ordered booth steps.
- Users can comment, like, dislike, and report strategies.
- Admin can hide reported content.
- Analytics record the core funnel.

## 11. Narrowest Viable Build Slice

Build the first vertical slice in this order:

1. Event and booth seed data.
2. Strategy browse and detail.
3. Wallet connect.
4. Paid unlock with mocked Stellar verification.
5. Real Stellar Testnet verification.
6. Started checklist with offline cache.

Do not start with admin dashboards, marketplace payout automation, or route optimization. The critical MVP proof is that a user can pay XLM and receive a useful offline strategy.
