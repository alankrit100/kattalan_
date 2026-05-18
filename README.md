# Kattalan_
# Thin Production MVP Engineering Blueprint
### Version 1.2 · Solo-Founder Optimized · Launch-First Architecture

---

# Executive Summary

SiteGuard is a lightweight website health monitoring SaaS focused on:

- SSL certificate monitoring
- uptime monitoring
- domain expiry monitoring
- weekly website health digest emails

The architecture is intentionally optimized for:

- rapid shipping
- solo-founder maintainability
- production-grade trust
- minimal operational complexity

This document intentionally preserves the production-grade thinking of the original architecture document while trimming premature scaling concerns from the MVP.

---

# Core Product Philosophy

Users do not buy monitoring.

Users buy:

```text
Peace of mind.
```

The product should feel:

- calm
- minimal
- reliable
- beautiful
- trustworthy

Avoid:

- noisy dashboards
- DevOps-heavy UX
- enterprise complexity
- unnecessary configuration

---

# Version 1 MVP Scope

```text
✓ Authentication
✓ Add/Delete Websites
✓ SSL Expiry Monitoring
✓ Website Uptime Monitoring
✓ Domain Expiry Monitoring
✓ Email Alerts
✓ Weekly Digest Emails
✓ Minimal Dashboard
✓ Stripe Billing
✓ Basic Plan Limits
✓ Rate Limiting
✓ SSRF Protection
✓ Sentry Error Tracking
✓ Monitoring SiteGuard itself
```

---

# Version 2 Roadmap

Moved intentionally out of MVP:

- Slack alerts
- Telegram alerts
- Historical uptime graphs
- Team collaboration
- White-label reports
- Status pages
- Multi-region monitoring
- Bull queues
- Redis orchestration
- AI summaries
- Browser automation checks

---

# Visual System Architecture

```text
                    ┌────────────────────┐
                    │    User Browser    │
                    └─────────┬──────────┘
                              │
                              ▼
                 ┌─────────────────────────┐
                 │ Next.js Frontend        │
                 │ Hosted on Vercel        │
                 └─────────┬───────────────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
 ┌────────────────┐ ┌──────────────┐ ┌─────────────┐
 │ Firebase Auth  │ │ Firestore DB │ │ Stripe API  │
 └────────────────┘ └──────────────┘ └─────────────┘
                           ▲
                           │
                           ▼
                ┌──────────────────────┐
                │ Railway Worker App   │
                │ Node.js + Cron Jobs  │
                └──────────┬───────────┘
                           │
        ┌──────────────────┼──────────────────┐
        ▼                  ▼                  ▼
   SSL Checker       Uptime Checker     Domain Checker
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ▼
                Alert & Digest Engine
                           ▼
                      Resend Emails
                           ▼
                        User Inbox
```

---

# Monitoring Lifecycle

```text
User adds website
        ↓
URL normalized + validated
        ↓
Stored in Firestore
        ↓
Cron workers pick up site
        ↓
SSL Check / Uptime Check / Domain Check
        ↓
Results stored in database
        ↓
Issue detected?
        ↓
YES → Alert Email
NO  → Wait for next cycle
        ↓
Included in Weekly Digest
```

---

# Final Tech Stack

## Frontend

- Next.js 14
- TailwindCSS
- shadcn/ui
- TanStack Query
- React Hook Form
- Zustand

## Backend

- Node.js 20
- node-cron
- axios
- ssl-checker
- whois npm
- Zod
- Pino logging

## Infrastructure

- Vercel
- Railway
- Firebase Auth
- Firestore
- Resend
- Stripe
- Sentry
- Plausible
- UptimeRobot

---

# Directory Structure

```text
siteguard/
├── apps/
│   ├── web/
│   └── workers/
├── packages/
│   ├── shared/
│   └── email-templates/
├── package.json
└── README.md

apps/web/
├── app/
├── components/
├── hooks/
├── lib/
└── middleware.ts

apps/workers/
├── jobs/
├── services/
├── utils/
└── index.ts
```

---

# Firestore Schema

```text
users/{userId}
  - email
  - plan
  - createdAt

users/{userId}/sites/{siteId}
  - url
  - label
  - ssl
  - uptime
  - domain
  - lastCheckedAt

users/{userId}/alerts/{alertId}
  - type
  - severity
  - sentAt
```

---

# Phase-Wise Implementation Plan

## DAY 1 — Foundation

- Monorepo setup
- Firebase auth
- Firestore schema
- Add/delete site flow
- Basic dashboard
- Deploy frontend

## DAY 2 — Monitoring

- SSL checks
- uptime checks
- domain expiry checks
- cron jobs
- Railway deployment
- structured logging

## DAY 3 — Emails

- Resend setup
- Alert emails
- Weekly digest
- Unsubscribe flow
- Email template testing

## DAY 4 — Billing + Polish

- Stripe integration
- Pricing page
- Plan limits
- Empty states
- Loading states
- Onboarding polish

## DAY 5 — Security + Launch

- Rate limiting
- SSRF protection
- Sentry setup
- SEO metadata
- Landing page
- Production deployment
- Launch preparation

---

# Critical Security Requirements

## NON-NEGOTIABLE

- Server-side Firebase JWT verification
- SSRF protection
- Zod validation on all APIs
- Protected API routes
- Stripe webhook verification
- Rate limiting
- CSP + CORS restrictions

### Blocked IP ranges

```text
127.x.x.x
10.x.x.x
172.16.x.x → 172.31.x.x
192.168.x.x
localhost
metadata endpoints
```

The biggest vulnerability in monitoring tools is SSRF.

Never allow monitoring internal/private networks.

---

# Reliability Principles

The biggest risk is NOT scaling.

The biggest risk is:

- missed alerts
- failed cron jobs
- unreliable email delivery

Because:

```text
TRUST = PRODUCT
```

Operational reliability matters more than advanced architecture in the MVP stage.

---

# Edge Cases

## SSL

- self-signed certificates
- invalid certificate chains
- expired certificates
- HTTP-only websites

## Uptime

- redirect loops
- timeout handling
- DNS failures
- 500 responses
- slow responses

## Domain

- GDPR-masked WHOIS
- unusual TLD formats
- redemption grace periods
- internationalized domains

---

# Testing Strategy

## Unit Testing

- URL normalization
- schema validation
- auth verification
- monitoring services

## Integration Testing

- API route protection
- adding/deleting sites
- billing flows
- alert generation

## Real-world monitoring tests

- github.com
- cloudflare.com
- expired.badssl.com
- self-signed.badssl.com
- neverssl.com

---

# Launch Strategy

## Launch Platforms

- Hacker News
- Reddit
- IndieHackers
- Twitter/X
- LinkedIn

## Core positioning

```text
Your website's Monday morning health report.
```

## Primary target

- small agencies
- freelancers
- indie founders
- developers managing client sites

---

# Pricing

```text
Free — 3 websites
Pro — $9/mo — 25 websites
Agency — $29/mo — Unlimited websites
```

---

# Success Metrics

## First 90 Days

- 200+ registered users
- 500+ monitored websites
- 20+ paying users
- 40%+ digest open rate
- 95%+ email deliverability

---

# Final Founder Guidance

Do not over-engineer.

Ship:

- small
- reliable
- beautiful
- focused

Avoid:

- premature scaling
- feature bloat
- infrastructure obsession

The goal is not perfect architecture.

The goal is getting real users to trust the product.

---

This document is intentionally structured as:

- product blueprint
- engineering roadmap
- architecture guide
- implementation contract

AI agents like Claude Code, Cursor, OpenClaude, or GPT-based coding agents can use this as high-context product understanding for implementation.
