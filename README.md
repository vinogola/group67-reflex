# Reflex

Group 67 · PLP 1MILL Devs Evaluation Phase · Week 3, "The Readiness Sprint"

A delivery coordination app for small Kenyan retailers. Three roles (Retailer, Dispatcher,
Rider) move an order from request to delivery. The differentiator is doubled scan
confirmation: the rider scans once at pickup, once at delivery, and each scan traces back to
the exact order and rider who made it.

> Built as a learning project (PLP), Group 67. Architecture is locked. The build hasn't
> started yet.

## System design

Full ERD and request-flow diagrams: [docs/system-design.md](docs/system-design.md)

## Team

| Role | Owner |
|---|---|
| Architecture Lead | Vincent Odhiambo |
| Demo Lead | Owen |
| Move-Forward Lead | Edith |
| Frame Lead | Edwin |

## Tech stack

Google AI Studio's Build mode, going with Cloud SQL for the relational database and Firebase
Authentication for the three roles. The data model needs five related tables, foreign keys,
one-to-many relationships running three different directions, and separate logins for three
roles - this isn't a single-user prototype. AI Studio also offers Firestore, but that's
document-based and would mean reshaping the locked ERD into collections instead of relational
tables.

## Status

Data model, ERD, and stack are locked. No code yet. The AI Studio build starts next.
Deadline Monday, 2026-08-31.
