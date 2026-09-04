# Reflex

Group 67 · PLP 1MILL Devs Evaluation Phase · Week 3, "The Readiness Sprint"

A delivery coordination app for small Kenyan retailers. Three roles (Retailer, Dispatcher,
Rider) move an order from request to delivery. The differentiator is doubled scan
confirmation: the rider scans once at pickup, once at delivery, and each scan traces back to
the exact order and rider who made it.

> Built as a learning project (PLP), Group 67. Architecture is locked. The AI Studio build
> is live with a working demo — Retailer, Dispatcher, and Rider flow through a full order
> end-to-end.

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

Google AI Studio's Build mode, going with Cloud SQL for the relational database. Auth is
Firebase Authentication with one account type and a role field (retailer, dispatcher,
rider) controlling what each user sees, not three separate login flows. The data model
needs five related tables, foreign keys, and one-to-many relationships running three
different directions - this isn't a single-user prototype. AI Studio also offers Firestore,
but that's document-based and would mean reshaping the locked ERD into collections instead
of relational tables.

## Status

Data model, ERD, and stack are locked. AI Studio build is live with a working demo:
https://ais-pre-k3jgpmphvtedm2g7s3pcxq-189625202835.europe-west2.run.app
(`docs/system-design.md` has both confirmed deviations, `Scan.scanned_at` and the `users`
auth-mapping table). Schema verified at both the API level and directly against the real
Cloud SQL schema. Auth is designed (one account + role field) but not yet implemented. The
demo's persona switching is currently unauthenticated, a known and disclosed limitation, not
an oversight. Trade-off log (`tradeoffs.md`) and demo script (`docs/demo-script.md`) are both
merged to `main`. Deadline is Saturday, 2026-09-05, 11:59 PM EAT. Submission is a recorded
walkthrough (group leader presents), not a live panel.
