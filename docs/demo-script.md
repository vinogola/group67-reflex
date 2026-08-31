# 🎬 Deliverable 4: Demo Script & Solution Walkthrough
**Owner:** Owen (Reveal / Solution & Demo Lead)  
**Team:** Group 67 (Reflex)  
**Live Demo Link:**   https://ais-pre-k3jgpmphvtedm2g7s3pcxq-189625202835.europe-west2.run.app

---

## 1. Persona 1: Retailer (Order Ingestion)
* **Goal:** Show order capture in seconds replacing WhatsApp messages.
* **Action:** Click "Retailer" tab -> Enter recipient (Amina Hassan, Mpaka Rd) -> Click "Log Delivery Request".
* **Script:**
  > "Nairobi retail delivery starts on informal WhatsApp groups where customer addresses get lost. With Reflex, store staff enter details and log directly into PostgreSQL, generating a unique tracking code and setting status to `requested`."
* **Database State:** `INSERT INTO "order"` (`status = 'requested'`, `retailer_id` linked).

---

## 2. Persona 2: Central Dispatch (Fleet Allocation)
* **Goal:** Real-time visibility & one-click assignment.
* **Action:** Click "Dispatcher" tab -> Select rider Vincent Otieno -> Click "Dispatch Rider".
* **Script:**
  > "Instead of calling riders one by one, the dispatcher sees unassigned requests alongside live fleet availability. Selecting Vincent and clicking dispatch links the order and locks Vincent’s status to `delivering` to prevent double-booking."
* **Database State:** `order.status -> 'assigned'`, `rider.status -> 'delivering'`.

---

## 3. Persona 3: Rider & Double-Scan (Our Locked Differentiator)
* **Goal:** Two-checkpoint proof of custody to eliminate lost parcel disputes.
* **Action:** Click "Rider" tab -> Execute Pickup Scan -> Execute Delivery Scan.
* **Script:**
  > "For the Rider, Reflex enforces a two-checkpoint double scan:
  > 1. Pickup Scan at the store logs an immutable timestamp in the `scan` table and marks the order `picked_up`.
  > 2. Delivery Scan upon customer handover marks the order `delivered` and resets the rider to `free` for the next dispatch."
* **Database State:** 2 rows in `scan` table (`pickup` & `delivery`), `order.status -> 'delivered'`, `rider.status -> 'free'`.

---

## 4. Cross-Examination Defense (State -> Context -> Evidence)
* **Q: Why scan at both pickup and delivery instead of just one?**
  * **State:** We require a scan at both points, not one.
  * **Context:** A single scan only proves one moment. With delivery-only, a rider could claim a pickup that never occurred; with pickup-only, there's no proof of delivery.
  * **Evidence:** Two independent scans verify both legs of the trip, eliminating the exact lack of proof found in WhatsApp and phone calls.
* **Q: What happens if a rider tries to scan delivery before pickup?**
  * **State:** The backend rejects out-of-order scans with an error.
  * **Context:** The order lifecycle follows a strict finite state machine: `requested` -> `assigned` -> `picked_up` -> `delivered`.
  * **Evidence:** A delivery scan requires the order to already be in `picked_up` status; otherwise, the API returns an HTTP 400 validation error.
