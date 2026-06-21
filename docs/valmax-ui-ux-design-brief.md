# Valmax MVP UI/UX Design Brief

## 1. Design Goal

Valmax should feel like a fast event companion, not a content platform. The main experience is: browse, judge, unlock if needed, start, follow checklist, report what happened.

The design should be optimized for one-handed mobile use, poor connectivity, crowded venue movement, and quick decisions.

## 2. UX Principles

- First screen shows the active event and the best available strategies.
- Strategy cards make trust and freshness visible.
- Paid content never feels like a blind purchase.
- Checklist mode is quiet, readable, and offline-ready.
- Feedback is lightweight: like, dislike, comment, report.
- Avoid game-like exploit language. Use "value", "hassle", "line", and "requirements".

## 3. Information Architecture

Primary tabs:
- Event
- Strategies
- Booths
- My Checklist
- Profile

Admin-only surfaces:
- Admin Events
- Admin Booths
- Admin Moderation
- Admin Revenue

## 4. Core Screens

### Event Home

Purpose:
- Orient attendee to the current event.
- Show event status, date, and quick entry points.

Content:
- Event name and live/ended state.
- Search.
- Featured strategies.
- Booth crowd highlights.
- Recently updated swag info.

Primary actions:
- Browse Strategies
- Browse Booths
- Connect Wallet

### Strategy Browse

Purpose:
- Help users quickly choose a route.

Strategy card content:
- Title.
- Creator.
- Free or XLM price.
- Number of booths.
- Estimated time.
- Value rating.
- Hassle rating.
- Likes/dislikes.
- Last updated.
- Preview text.
- Thumbnail image if available.

Filters:
- Free / Paid.
- Short route / Full route.
- High value.
- Low hassle.
- Recently updated.

Sorting:
- Trending.
- Newest.
- Most liked.
- Lowest hassle.

### Strategy Detail

Purpose:
- Let user judge whether the strategy is worth starting or paying for.

Visible before unlock:
- Title.
- Creator.
- Price if paid.
- Summary.
- Preview image.
- Booth count.
- Estimated time.
- First one or two steps, if creator allows.
- Likes/dislikes.
- Recent comments or warnings.
- Last updated.
- Safety note: "Strategies should follow event and booth rules."

Locked paid content:
- Full strategy text hidden.
- Full step list hidden.
- Start button disabled until unlock.

Actions:
- Unlock with XLM.
- Start if free/unlocked.
- Like/dislike.
- Comment.
- Report.

### Payment Confirmation

Purpose:
- Keep XLM payment understandable and trustable.

Content:
- Strategy title.
- Creator.
- Total price in XLM.
- Platform fee.
- Creator share.
- Destination address label.
- Network: Testnet for staging, Mainnet for production.

Actions:
- Connect wallet.
- Sign payment.
- Cancel.

States:
- Waiting for signature.
- Submitted.
- Confirming on Stellar.
- Confirmed.
- Failed.
- Timed out.

### Checklist Mode

Purpose:
- Give a low-distraction offline route.

Content:
- Strategy title.
- Offline status indicator.
- Progress count.
- Ordered booth steps.
- Each step includes booth name, action, requirement, expected swag, fallback if line is long.

Actions:
- Check step done.
- Add quick note.
- Comment on strategy.
- Report issue.
- Refresh when online.

Offline behavior:
- Once user presses Start, cache a strategy snapshot and checklist state.
- Display "Saved for offline" when local cache succeeds.
- If offline, show cached content and queue progress sync.

### Booth Detail

Purpose:
- Show live booth value and friction.

Content:
- Sponsor/booth name.
- Official/admin-verified badge if applicable.
- Swag offers.
- Requirements.
- Availability.
- Crowd/wait reports.
- Related strategies.
- Comments.

Actions:
- Submit line report.
- Update swag info.
- Like/dislike info.
- Comment.
- Report.

### Strategy Creation

Purpose:
- Let attendees publish useful text and image-based strategies without complex tooling.

Steps:
- Title and summary.
- Free or paid.
- Price in XLM if paid.
- Upload images.
- Add ordered booth steps.
- Add fallback notes.
- Confirm rule-following attestation.
- Preview.
- Publish.

Design constraint:
- The strategy builder should be step-by-step, not a giant form.

### Comments And Feedback

Comment prompts:
- "Worked for me"
- "Line was too long"
- "Swag was gone"
- "Requirement changed"
- "Other"

Reaction controls:
- Thumbs up for useful.
- Thumbs down for not useful.

Moderation:
- Report visible but secondary.
- Admin can hide content.

## 5. Visual Direction

Tone:
- Fast, credible, event-native, utility-first.

Recommended palette:
- Near-white or deep charcoal base.
- High-contrast text.
- Accent colors for status only:
  - Green: available/done.
  - Amber: limited/wait.
  - Red: gone/problem.
  - Blue: verified/payment.

Avoid:
- One-note purple/crypto gradient styling.
- Heavy decorative cards.
- Marketing landing-page hero layout.
- Tiny dense controls that fail in a moving crowd.

Components:
- Bottom tab navigation.
- Sticky primary action on strategy detail.
- Compact strategy cards with clear metadata.
- Checklist rows with stable checkbox hit areas.
- Segmented filters.
- Icon buttons with labels or tooltips where needed.
- Status badges for free, paid, verified, updated, offline saved.

## 6. Mobile Requirements

- All primary actions reachable with thumb at bottom of screen.
- Minimum tap target of 44px.
- Text must not overflow on narrow phones.
- Strategy cards must remain scannable at 360px width.
- Checklist rows must not resize when checked.
- Payment confirmation must fit without horizontal scrolling.
- Offline banner must not cover checklist actions.

## 7. Empty And Error States

No strategies yet:
- Show booths and invite connected users to create the first strategy.

No wallet:
- Explain that wallet connection is required for premium unlocks and posting.

Payment failed:
- Show no blame. Offer retry, view transaction if available, and contact support/report issue.

Offline:
- Show cached checklist if available.
- Disable actions requiring server confirmation.
- Queue local progress updates.

Strategy removed:
- Show unavailable message and refund/dispute instructions if paid.

## 8. UX Acceptance Criteria

- A new user can understand the app purpose within 10 seconds of event home.
- A user can reach strategy detail in 2 taps or fewer from event home.
- A user can identify whether a strategy is free or paid from the strategy card.
- A user can see strategy freshness before unlocking.
- A user can start a free strategy without payment.
- A user can unlock a paid strategy without leaving the flow except for wallet signing.
- A user can complete a checklist step with one tap.
- A user can use an already started checklist while offline.
- A user can comment on success or problems after using a strategy.
- Admin moderation actions are not visible to regular attendees.
