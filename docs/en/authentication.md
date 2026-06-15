# Authentication

## Overview

Authentication in Tuttofy core web controls how learners, tutors, and family account members create accounts, verify identity, start sessions, and enter the right product context safely. Tuttofy uses Clerk as the full authentication provider while keeping a custom auth UI in Next.js for the product experience.

## Purpose

This feature exists to ensure that only verified users can access the Tuttofy core product, that identity is managed consistently across email and Google login methods, and that internal application profiles, family memberships, and learning contexts can be connected to a reliable user record before onboarding or product access begins.

## Users / Roles

- Learner
- Tutor
- Family account member
- Internal product and engineering teams that rely on the identity model

## Main Flow

1. A new user opens the shared sign-up entry point in the Tuttofy core web app.
2. The user chooses either passwordless email OTP or Google as the authentication method.
3. If the user uses email OTP, Clerk sends the verification code to the user's email and the user enters the code in Tuttofy's custom auth UI.
4. If the user uses Google, Clerk completes the Google sign-in flow and returns the authenticated identity to Tuttofy.
5. Tuttofy requires a verified email before granting application access.
6. After the first verified sign-in succeeds, Tuttofy creates or completes the matching internal user or profile record in Neon if needed.
7. Tuttofy checks whether the user's email has a family membership, an active family invite, or an existing personal context.
8. If the user does not yet have a complete internal profile, the user is routed into onboarding before using the main product experience.
9. If the user has more than one context, such as `personal` and `family`, Tuttofy shows context selection or `switch to family` before entering the right app area.
10. On later visits, the same user signs in again with email OTP or Google and Clerk restores the active session.
11. When the user signs out, Clerk ends the session and Tuttofy returns the user to an unauthenticated state.

## Visual Flow

```mermaid
flowchart TD
  start[User opens auth entry point] --> method{Choose auth method}
  method -->|Email OTP| otp[Clerk sends OTP]
  method -->|Google| google[Clerk runs Google sign-in]
  otp --> verify[User completes verification]
  google --> verify
  verify --> access{Email verified?}
  access -->|No| denied[Stay blocked from app access]
  access -->|Yes| sync[Sync or create internal Neon profile]
  sync --> membership[Check personal and family context]
  membership --> onboarding[Route to onboarding if profile is incomplete]
  membership --> chooser[Show context choice if needed]
  onboarding --> chooser
  chooser --> session[Active authenticated session]
```

## Interaction Sequence

```mermaid
sequenceDiagram
  actor User
  participant App as Tuttofy Web
  participant Clerk
  participant Neon
  participant Invite as Family Membership
  User->>App: Open sign-up or sign-in page
  App->>Clerk: Start OTP or Google auth flow
  Clerk-->>User: Request verification step
  User->>Clerk: Submit OTP or complete Google login
  Clerk-->>App: Return verified identity and session
  App->>Neon: Find or create internal user profile
  Neon-->>App: Return profile and onboarding state
  App->>Invite: Check membership or active invites
  Invite-->>App: Return available contexts
  App-->>User: Continue to onboarding, context choice, or app
```

## Business Rules

- `Clerk` fully handles sign-up, sign-in, verification, and session lifecycle for the Tuttofy core web app.
- Tuttofy uses custom authentication screens in `Next.js`, not Clerk-hosted default pages.
- The allowed authentication methods in this phase are only `passwordless email OTP` and `Google`.
- `Google` is the only supported social provider in this phase.
- A user must complete email verification before accessing the application.
- A verified email represents one account identity even if the same user signs in through both email OTP and Google.
- One identity can have multiple product contexts such as `personal` and `family`.
- Family membership belongs to the Tuttofy application domain, not the Clerk identity domain.
- A verified email that is already registered can still receive an invite to a family account.
- If an invite email already has an active account, the user does not create a new account and only adds family membership to the same identity.
- Family billing belongs to the `family owner`, but detailed billing rules are documented in the `family-account` feature, not on this auth page.
- `Clerk` is the source of truth for identity and authentication state.
- `Neon` is the source of truth for internal application data, family membership, and user context after a user is recognized by Clerk.
- `clerk_user_id` is the primary link between Clerk identity and Tuttofy internal records.
- Onboarding begins only after the first verified sign-in succeeds.
- Authentication only establishes who the user is and which context is active. It does not determine learning paths or course enrollment.
- Admin authentication is out of scope for this flow because the admin system lives in a separate application.
- Payment or subscription details are outside the scope of this auth page, but teacher wallet/payout uses `clerk_user_id` to link identity to the wallet record and Stripe connected account.

## Data / Fields

- `clerk_user_id`
- `primary_email`
- `email_verification_status`
- `auth_provider`
- `connected_providers`
- `session_status`
- `available_contexts[]`
- `active_context`
- `family_membership_status`
- `pending_family_invites[]`
- `onboarding_status`
- `internal_user_profile_id`

## Edge Cases

- The entered OTP is incorrect.
- The OTP has expired and the user must request a new code.
- The email is already associated with an existing account.
- The user signs in with Google using the same verified email that was previously used for email OTP.
- The user receives a family invite on an email that has never registered, then completes sign-up.
- The user receives a family invite on an email that already has an account, then must see the `switch to family` option.
- Clerk authenticates the user but Neon does not yet have the matching internal user or profile record.
- The user attempts to access the app before email verification is complete.
- The user signs out during onboarding and later returns to finish the remaining setup.
- The user has more than one active context and needs to choose the right context before entering the product.

## Related Features

- Onboarding
- Family account
- User profile
- Teacher profile
- Join course
- Teacher wallet
- Tech Stack

## Notes

- Password-based local authentication is not part of the current Tuttofy core web scope.
- Tutor, learner, and family member accounts use the same authentication entry point and diverge later through onboarding, membership context, profile state, or permissions.
- The current auth documentation scope does not include internal admin login, non-Google social providers, or detailed payment and subscription flows.
