# BookMe — AI-Powered Multi-Vendor Event Service Booking Platform

BookMe connects customers with event service vendors (photography, catering, DJ, decoration, makeup, lighting, transport and more) for weddings, birthdays, engagements, anniversaries and corporate events.

## Problem Statement
Organising an event means contacting many vendors separately, comparing prices by hand, and being unsure about availability and reliability. BookMe puts event creation, vendor discovery, comparison, booking, payment and event management on one platform.

## Main Users and Features
| User | Key features |
|------|--------------|
| Customer | Register/login, create event, search/filter/compare vendors, check availability, book, pay, budget, checklist, event dashboard, reviews |
| Vendor | Register, verification, profile, services, packages, availability, booking requests (accept/reject), earnings |
| Admin | Vendor verification, manage users/vendors/bookings/payments, handle complaints and refunds |

Planned later: AI event planner, AI recommendations, natural-language search, review summaries (V2); emergency vendor replacement, mobile app, multi-city (V3).

## Architecture Overview
Microservice-oriented: React frontend → API gateway → four services. **Each service owns its own PostgreSQL database; no service accesses another's database directly.** Services use REST/JSON and RabbitMQ events.

| Service | Responsibility |
|---------|----------------|
| `auth-service` | Login/register, JWT, roles, user profile |
| `vendor-service` | Vendor profile, verification, services, packages, availability |
| `event-booking-service` | Events, dashboard, budget, checklist, booking lifecycle |
| `payment-feedback-service` | Payments, payouts, reviews, complaints |

## Technology Stack (planned)
React, Node.js/Express, PostgreSQL (one DB per service), REST + JSON, JWT, RabbitMQ, Docker/Docker Compose.

## Repository Structure
```
bookme/
├── README.md
├── brief.md
├── http-log.md
├── network-analysis.md
├── docs/
│   ├── assignment-1.md
│   └── http-testing-guide.md
├── frontend/
├── services/
│   ├── auth-service/
│   ├── vendor-service/
│   ├── event-booking-service/
│   └── payment-feedback-service/
├── contracts/
└── .gitignore
```

## How to Run
There is **no runnable application code yet**; this repository currently holds Assignment 1 documentation and the project skeleton. The only verified commands are the HTTP demonstrations in [http-log.md](http-log.md), e.g.:
```bash
curl -i https://registry.npmjs.org/express/latest
```
Application setup commands will be added once the services are implemented.

## Assignment 1 Deliverables
| Deliverable | File |
|-------------|------|
| Project overview | [README.md](README.md) |
| HTTP by hand with curl | [http-log.md](http-log.md) |
| DevTools network analysis (template, needs your measurements) | [network-analysis.md](network-analysis.md) |
| Project brief | [brief.md](brief.md) |

Supporting: [docs/assignment-1.md](docs/assignment-1.md), [docs/http-testing-guide.md](docs/http-testing-guide.md).

## Current Implementation Status and Limitations
- ✅ Documentation, repository skeleton, real curl log.
- ⏳ `network-analysis.md` must be completed from a real browser.
- ❌ Not implemented: frontend, services, databases, gateway, RabbitMQ, AI features, Docker Compose.
- API paths shown in `brief.md` are **proposed contracts, not live endpoints**.
