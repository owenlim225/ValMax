# Valmax Gate Review

## 1. Maestro Result

Current situation:
- Product idea is clarified enough for MVP planning.
- The target market is crypto/Web3 events.
- The primary proof point is Stellar XLM payment.
- Implementation should not start until product, architecture, evidence, logic, safety, UX, and build scope are coherent.

Best next step completed:
- Move through `$arch -> $sage -> $flow -> $vet -> $vibe -> $build`, then apply `cleanup`.

Avoid doing yet:
- Do not build custom tokens, smart contracts, native apps, map routing, or automated payout contracts in MVP.

## 2. Align Result

Normalized terms:
- Event: admin-created Web3 convention/summit.
- Booth: sponsor/exhibitor entry.
- Swag offer: booth reward and requirement.
- Line report: user-submitted crowd/wait update.
- Strategy: text/image post with ordered booth steps.
- Checklist: offline-ready started strategy.
- Unlock: confirmed XLM payment granting access.

Scope boundary:
- In scope: event browse, booth data, strategy posts, XLM unlock, offline checklist, comments/reactions, moderation.
- Out of scope: native apps, map routing, custom token, NFT rewards, automated route engine, organizer portals.

Decision:
- `idea_status = CLEAR`

## 3. Arch Result

Problem framing:
- Attendees at Web3 events need fast, trustworthy guidance on which booths are worth visiting and in what order.

Architecture:
- Next.js mobile web app.
- Supabase Postgres backend.
- Stellar wallet connection and XLM payment verification.
- Service-worker-based offline checklist support.
- Admin-created events and booths.

Boundaries:
- Blockchain is used for payment proof, not for storing strategies.
- Database is source of truth for content, access grants, moderation, and checklist state.
- Stellar transaction is source of truth for payment confirmation.

Top risks:
- Wallet friction on mobile.
- Paid stale strategies.
- Fake crowd reports.
- Low strategist supply.
- Unlock/payment edge cases.

Decision:
- `architecture_status = APPROVED`

## 4. Sage Result

Verified evidence:
- Stellar docs support JavaScript payment app patterns, Freighter wallet integration, transaction signing, and payment examples.
- MDN supports service workers/PWA offline behavior as the correct web primitive.
- Supabase docs support RLS as a browser-safe authorization layer when exposing data APIs.
- Next.js docs support App Router as a modern React app structure.

Recommendation:
- Use XLM payment operations and server verification for MVP.
- Use wallet-based identity.
- Use database access grants for unlocked content.
- Use service workers plus IndexedDB/local persistence for offline checklist snapshots.

Unresolved evidence gaps:
- Final production wallet support beyond Freighter should be tested on the target attendees' devices.
- Stellar Mainnet transaction verification approach should be load-tested before a real event.

Decision:
- `evidence_status = VERIFIED`

## 5. Flow Result

Actors:
- Guest.
- Connected attendee.
- Strategist.
- Admin.
- Stellar wallet.
- Stellar network.

Main states:
- Event: draft, live, ended, archived.
- Strategy: draft, published, hidden, removed.
- Unlock: pending, confirmed, failed, refunded.
- Checklist: not started, started local, synced.
- Payment intent: created, signed, submitted, verified, expired, rejected.

Main flows:
- Browse free preview.
- Connect wallet.
- Unlock paid strategy.
- Start checklist.
- Complete booth steps.
- Comment/react.
- Create strategy.
- Admin moderate.

Failure flows:
- Wallet not installed: show install/help state.
- User rejects signature: return to strategy detail without unlock.
- Payment submitted but not confirmed: keep pending and allow retry verification.
- Wrong amount/destination/memo: reject unlock.
- Offline before start: no cached checklist available.
- Offline after start: show cached checklist and queue changes.
- Strategy hidden after purchase: show unavailable and admin support path.

Invariants:
- Paid full content is visible only after confirmed unlock.
- Client cannot mark payment confirmed.
- One transaction hash cannot unlock multiple purchases.
- Checklist snapshot persists after Start.
- Strategy comments and reactions do not alter paid access.
- Admin-created event and booth records are canonical for MVP.

Decision:
- `logic_status = GREEN`

## 6. Vet Result

Trust boundaries:
- Browser to backend API.
- Backend to database.
- Backend to Stellar network.
- Browser wallet to signed transaction.
- Offline local cache to sync API.
- User-generated content to public display.
- Admin moderation to public state.

Sensitive data:
- Wallet public keys.
- Payment transaction hashes.
- Unlock records.
- Strategy purchases.
- Local cached premium strategy content.
- Admin credentials.

Material abuse cases:
- Fake paid unlock from client.
- Reusing a transaction hash.
- Fake line reports to manipulate traffic.
- Paid strategy with misleading claims.
- Rule-breaking strategy advice.
- Harassment or spam comments.
- Scraping premium content after unlock.
- XSS through user text or uploaded images.

Must-fix mitigations:
- Server-side payment verification only.
- Unique payment intent memo/reference.
- Unique transaction hash constraint.
- Sanitize user-generated text.
- Restrict image uploads by type and size.
- RLS/authorization policies for all user-owned rows.
- Report and admin-hide flows.
- Event-rules attestation before publish.
- Rate limits for comments, reports, line reports, and strategy creation.
- Do not store wallet private keys.

Accepted MVP risks:
- Premium content can be screenshotted or copied after unlock.
- Crowd reports may be imperfect.
- Strategist payouts may be manual during validation.

Decision:
- `security_status = GREEN` if must-fix mitigations are implemented.

## 7. Vibe Result

Developer workflow risks:
- Payment verification can become tangled with UI if not isolated.
- Offline sync can sprawl if attempted globally.
- Admin moderation can overgrow the first build.

User workflow risks:
- Wallet install/connect may interrupt event usage.
- Strategy unlock may feel risky if preview quality is weak.
- Offline access must be explicit before the attendee walks away.

Simplifications:
- One event at a time for MVP.
- One payment asset: XLM.
- One receiving account.
- Manual payout ledger first.
- Offline only for started checklists, not the whole app.
- Admin-created events only.

Decision:
- `experience_status = GREEN`

## 8. Build Result

Current readiness verdict:
- Ready for implementation planning and first vertical slice.

Blockers:
- None blocking MVP spec.
- Before production launch, choose final Stellar Mainnet account handling, fee percentage, and refund/dispute policy.

Narrowest viable build slice:
- Event browse.
- Strategy preview/detail.
- Wallet connect.
- Paid XLM unlock.
- Offline started checklist.

Test expectations:
- Payment edge cases.
- Access control.
- Offline checklist.
- Mobile layout.
- Moderation visibility.

Exact next implementation step:
- Scaffold the app, schema, seed event data, and first strategy unlock/checklist vertical slice.

Decision:
- `build_readiness = READY_FOR_VERTICAL_SLICE`

## 9. Cleanup Result

Removed complexity:
- Custom token.
- Smart contract marketplace.
- Native mobile app.
- Indoor routing.
- Automated optimization.
- Fully decentralized storage.
- Organizer self-service.

Preserved behavior:
- Free preview.
- XLM premium unlock.
- Text/image strategies.
- Ordered booth checklist.
- Offline after start.
- Comments, likes, dislikes.
- Admin-created events and booths.
- Safety boundary against exploit guidance.

Residual risks:
- Wallet friction at the event.
- Strategy quality depends on creator incentives.
- Offline cache requires careful testing on mobile browsers.
- Paid strategy disputes need an operational policy.
