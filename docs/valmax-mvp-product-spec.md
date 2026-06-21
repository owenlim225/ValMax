# Valmax MVP Product Spec

## 1. Product Summary

Valmax is a mobile-first community web app for crypto and Web3 events. It helps attendees spend less time guessing, waiting, and walking by showing booth swag offers, requirements, crowd reports, and user-created booth-order strategies.

The MVP proof point is Stellar payment utility: users can browse event previews for free, then unlock premium strategies with XLM through their own Stellar wallet.

## 2. Positioning

Valmax is the live swag strategy layer for Web3 events.

For crypto event attendees who have limited time and want the highest-value booth route, Valmax combines crowd-sourced booth intelligence with paid or free strategy checklists that can be started, saved, and used offline.

## 3. Target User

Primary user:
- A crypto/Web3 event attendee with a Stellar wallet or willingness to connect one.
- Goal: collect worthwhile merch while avoiding long lines, low-value booths, and unclear requirements.

Secondary user:
- A strategist who knows the event floor and can publish a route.
- Goal: earn XLM, sponsorships, reputation, or visibility.

Admin user:
- Valmax platform admin.
- Goal: create events, add sponsors/booths, moderate content, and keep the first event useful.

## 4. Core Problem

At events, attendees lose time because swag information is scattered, line conditions change, and booth ROI is unclear. Existing event websites usually list sponsors, not practical attendee strategy. Valmax turns booth and swag information into ordered, actionable routes.

## 5. Market Stress Test

### Why It Could Work

- Crypto/Web3 audiences already understand wallets and token payments, lowering payment onboarding friction compared with general conventions.
- Event swag is time-sensitive and scarce, which makes live strategy more valuable than a static event map.
- Strategists can create high-leverage content from local knowledge: "go here first, skip this, do this only if line is short."
- The product fits a narrow, testable wedge: one Web3 event, one payment rail, one strategy unlock flow.

### Why It Could Fail

- Event attendees may not want to pay before trust exists. Free preview quality must prove value fast.
- Paid strategies can become stale during the event. The product must make recency and comments visible.
- Wallet-required flows may still create friction on mobile, especially if the attendee has wallet access only on desktop.
- Strategy creators may not produce useful content unless rewards, recognition, and posting UX are simple.
- Moderation matters: bad advice, fake line reports, and misleading paid strategies can destroy trust quickly.

### MVP Validation Hypothesis

Valmax is worth building if, during one crypto/Web3 event:
- At least 30 percent of connected-wallet users start a free or paid strategy.
- At least 10 percent of connected-wallet users unlock at least one premium strategy with XLM.
- At least 25 percent of started strategies receive a checklist completion, comment, like, or dislike.
- Median user-reported value is positive: "This saved me time or helped me choose booths."

## 6. Business Model

MVP monetization:
- Free preview: event page, sponsor/booth list, public booth offer summaries, public strategy previews.
- Premium strategy unlock: attendee pays XLM to access full text, images, and ordered checklist.
- Platform fee: small fee retained from premium strategy payments.
- Strategist payout: remaining XLM owed to strategist, initially tracked in app and settled manually or through a simple admin payout flow.

Optional but not required for MVP:
- Sponsorship/tipping for free strategies.
- Featured strategist placements.
- Event organizer subscription.

MVP pricing guidance:
- Keep unlock amounts low enough for impulse use at an event.
- Show total XLM, platform fee, and strategist share before wallet signing.
- Support free strategies so the app is still useful before a paid marketplace emerges.

## 7. Product Principles

- Time saved beats content volume.
- Strategies must be usable while walking.
- No exploit guidance: only maximize value within booth and event rules.
- Make recency visible.
- Let the crowd challenge strategy quality through comments, likes, and dislikes.
- Offline checklist access is part of the core value, not a nice-to-have.

## 8. MVP Scope

### In Scope

- Mobile-friendly web app.
- Platform-admin-created events.
- Admin-added sponsor/booth directory.
- Booth swag offers, requirements, estimated value, and current crowd/wait reports.
- User-submitted booth info updates.
- User-created strategies with text, images, ordered booth steps, price, and free/paid visibility.
- Strategy preview pages.
- XLM payment unlock for premium strategies.
- Wallet connect using a Stellar wallet such as Freighter.
- Started strategy checklist with local/offline availability.
- Comments, likes, and dislikes on strategies and information.
- Basic reporting/moderation.
- Admin moderation dashboard.

### Out Of Scope

- Native iOS/Android app.
- Indoor maps or turn-by-turn navigation.
- Complex route optimization.
- Custom Stellar token.
- NFTs.
- Fully decentralized storage.
- Real-time chat.
- Organizer self-service portals.
- Automated strategist payout smart contracts.
- Multi-event marketplace.

## 9. Roles And Permissions

Guest:
- Can browse public event preview.
- Can see sponsor/booth list and strategy previews.
- Cannot unlock, post, comment, like, dislike, or start a private strategy without connecting wallet.

Connected attendee:
- Can unlock premium strategies with XLM.
- Can start strategies and download/cache checklists.
- Can comment, like, dislike, and submit booth info updates.
- Can create free or paid strategies.

Strategist:
- Same as connected attendee.
- Owns created strategies.
- Can edit unpublished strategies.
- Can update published strategies with visible "updated at" metadata.

Admin:
- Creates events.
- Adds sponsors/booths.
- Moderates strategies, comments, and reports.
- Marks booth/info entries as admin-verified.
- Manages premium unlock disputes and payout records.

## 10. Core Objects

Event:
- id
- name
- venue
- dates
- timezone
- status: draft, live, ended, archived
- description
- hero image
- created by admin

Sponsor/Booth:
- id
- event id
- sponsor name
- booth name or number
- category
- description
- official URL
- admin verified flag

Swag Offer:
- id
- booth id
- item name
- item type
- requirement
- estimated value: low, medium, high
- hassle level: low, medium, high
- availability: unknown, available, limited, gone
- last updated at
- source: admin, user, strategist

Line Report:
- id
- booth id
- reporter wallet/user id
- crowd level: empty, short, medium, long, packed
- estimated wait minutes
- note
- created at

Strategy:
- id
- event id
- creator id
- title
- summary
- full text
- images
- ordered booth steps
- price XLM
- visibility: free, paid
- status: draft, published, hidden, removed
- safety flag: follows event rules
- likes count
- dislikes count
- comments count
- updated at

Strategy Step:
- id
- strategy id
- position
- booth id
- action
- rationale
- expected swag
- fallback if line is long

Unlock:
- id
- strategy id
- buyer id
- buyer public key
- amount XLM
- platform fee XLM
- strategist share XLM
- Stellar transaction hash
- payment status: pending, confirmed, failed, refunded
- created at

Started Checklist:
- id
- user id
- strategy id
- cached strategy snapshot
- checked step ids
- started at
- last local update at
- sync status: local only, synced

Comment:
- id
- target type: strategy, booth, swag offer
- target id
- author id
- body
- rating context: success, problem, update, warning
- created at
- moderated status

Reaction:
- id
- target type
- target id
- author id
- reaction: like, dislike

Report:
- id
- target type
- target id
- reporter id
- reason
- status: open, reviewed, actioned, dismissed

## 11. User Journey

1. Attendee lands on event page.
2. Attendee browses sponsor/booth directory and strategy previews.
3. Attendee connects Stellar wallet.
4. Attendee chooses a strategy.
5. If free, attendee starts it immediately.
6. If paid, attendee reviews price and signs an XLM payment.
7. App verifies payment and unlocks strategy.
8. Attendee presses Start.
9. App saves the ordered booth checklist for offline use.
10. Attendee completes booth steps and checks them off.
11. Attendee comments about success, blockers, stale info, or line issues.
12. Other users use likes/dislikes and comments to judge quality.

## 12. Safety Model

Allowed strategy guidance:
- Which booth has better swag.
- Which booth has higher/lower hassle.
- Which booth has long lines.
- Which order may save time.
- Which booth to skip because value is low or requirements are unclear.

Not allowed:
- Rule bypassing.
- Harassment or pressure toward staff.
- Misrepresentation to claim swag.
- Hoarding or repeat-claim advice when against booth rules.
- Physical safety risks such as running, crowd manipulation, or obstructing lines.
- Fraudulent payment or unlock workarounds.

MVP enforcement:
- Strategy creation requires a checkbox: "This strategy follows event and booth rules."
- Users can report strategies or comments.
- Admin can hide/remove content.
- Comments and dislikes remain visible signals unless moderated.
- Paid strategies show "last updated" and comments before unlock when possible.

## 13. Acceptance Criteria

Product acceptance:
- A guest can view one live event, booth list, and strategy previews without payment.
- A connected wallet user can unlock a premium strategy with XLM.
- A successful payment records a transaction hash and changes access from locked to unlocked.
- A user can start an unlocked strategy and see an ordered checklist.
- A started checklist remains usable after network is disabled and the page is refreshed.
- A user can check off steps, then later reconnect and sync progress.
- A user can comment, like, or dislike a strategy.
- An admin can create an event and add booths/sponsors.
- An admin can hide a strategy, comment, or report-abused content.
- A paid strategy clearly displays price, creator, updated date, and preview before payment.

Safety acceptance:
- Strategy creation requires an event-rules attestation.
- Report controls are visible on strategy and comment surfaces.
- Hidden/removed content is not visible to regular users.
- Users cannot edit another user's strategy, comment, reaction, unlock, or checklist.
- Payment status cannot be set to confirmed by the client alone.

Business acceptance:
- Platform fee is calculated and stored for each paid unlock.
- Admin can export or inspect strategist revenue owed.
- Analytics capture browse, wallet connect, unlock attempt, unlock success, start strategy, checklist complete, comment, like, and dislike events.

## 14. Sources Used For Technical Grounding

- Stellar payment app tutorial: https://developers.stellar.org/docs/build/apps/example-application-tutorial/overview
- Stellar Freighter wallet docs: https://developers.stellar.org/docs/build/guides/freighter
- Stellar Freighter React integration: https://developers.stellar.org/docs/build/guides/freighter/integrate-freighter-react
- Stellar Freighter signing docs: https://developers.stellar.org/docs/build/guides/freighter/prompt-to-sign-tx
- Stellar send and receive payments: https://developers.stellar.org/docs/build/guides/transactions/send-and-receive-payments
- Stellar Horizon API reference: https://developers.stellar.org/docs/data/apis/horizon/api-reference
- Stellar security best practices: https://developers.stellar.org/docs/build/security-docs
- MDN Service Worker API: https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- MDN PWA offline/background guide: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Offline_and_background_operation
- Supabase Row Level Security docs: https://supabase.com/docs/guides/database/postgres/row-level-security
- Next.js App Router docs: https://nextjs.org/docs/app
