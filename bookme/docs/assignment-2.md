# Assignment 2 — Design BookMe: Services, Contracts & Schema

**CS 543 Web Services** · Assignment 02 of 14  
**Project:** BookMe (AI-Powered Multi-Vendor Event Service Booking Platform)  
**Team 11:**
- **Radhika Verma** (`20251651075`) — Lead: Auth Service (`auth-service`)
- **Nandini Kanaujiya** (`20251651061`) — Lead: Vendor Service (`vendor-service`)
- **Alka Jha** (`20251651013`) — Lead: Event & Booking Service (`event-booking-service`) *(Central Engine)*
- **Alok Mishra** (`20251651014`) — Lead: Payment & Feedback Service (`payment-feedback-service`)

---

## 📌 Deliverables Checklist

| Task | Deliverable | File in Repo | Status |
| :--- | :--- | :--- | :--- |
| **Task 1: Capabilities** | 7 Verbs mapped from `brief.md` to BookMe services | [`design.pdf`](../assignment-2/design.pdf) | ✅ Done |
| **Task 2: Service Benchmark** | Microservice Architecture Diagram with boundaries & team members | [`services.drawio`](../assignment-2/services.drawio) / [`services.png`](../assignment-2/services.png) | ✅ Done |
| **Task 3: Contracts** | API Operation Contracts (Inputs, Outputs, Errors, No SQL) | [`design.pdf`](../assignment-2/design.pdf) | ✅ Done |
| **Task 4: Central Operation** | Full specification of `createBooking` (`POST /bookings`) | [`design.pdf`](../assignment-2/design.pdf) | ✅ Done |
| **Task 5: Database Schema** | Relational ERD & PostgreSQL DDL (Zero Shared Tables) | [`schema.drawio`](../assignment-2/schema.drawio) / [`schema.png`](../assignment-2/schema.png) / [`schema.sql`](../assignment-2/schema.sql) | ✅ Done |
| **Task 6: Validation** | 5 Service Properties audit with trade-offs & remediations | [`design.pdf`](../assignment-2/design.pdf) | ✅ Done |
| **Submission Brief** | Complete report with GitHub & Google Drive video demo links | [`ReadMe.pdf`](../../ReadMe.pdf) / [`assignment-2/README.md`](../assignment-2/README.md) | ✅ Done |

---

## 🛠️ Mapping to BookMe Codebase & Architecture

### 1. Capabilities (The Verbs) — How They Map to BookMe
In Assignment 1 (`brief.md` § *Verbs — Actions and Operations*), we outlined the core jobs BookMe performs. For Assignment 2, each capability is mapped to a dedicated microservice:

1. **`registerAndAuthenticateUser`** (`auth-service` / `POST /auth/register`, `POST /auth/login`):
   - **Where in BookMe:** Located in `services/auth-service/src/controllers/authController.js`.
   - **Job:** Creates user records, verifies email uniqueness, hashes passwords with bcrypt/salt, and issues signed JWT bearer tokens for role-based access control (CUSTOMER, VENDOR, ADMIN).
2. **`manageVendorCatalog`** (`vendor-service` / `GET /vendors`, `POST /vendors`):
   - **Where in BookMe:** Located in `services/vendor-service/src/modules/vendor/controllers/vendorController.js`.
   - **Job:** Onboards vendor business identities, catalogs specialized offerings (catering, photography, decoration, DJ), and structures priced bundles.
3. **`manageCalendarAvailability`** (`vendor-service` / `GET /vendors/{id}/availability`, `POST /availability/block`):
   - **Where in BookMe:** Located in `services/vendor-service/src/modules/availability/controllers/availabilityController.js`.
   - **Job:** Provides calendar slot querying and date reservation locks, ensuring no vendor receives overlapping bookings on the same event date.
4. **`planEventAndBudget`** (`event-booking-service` / `POST /events`, `GET /events/{id}/checklist`):
   - **Where in BookMe:** Located in `services/event-booking-service/src/modules/event/`.
   - **Job:** Manages the customer's event container (guest count, target date, budget ceiling) and tracks milestone checklists.
5. **`createBooking`** (`event-booking-service` / `POST /bookings`):
   - **Where in BookMe:** Located in `services/event-booking-service/src/modules/booking/` (Central Operation).
   - **Job:** Validates customer ownership, coordinates downstream calendar availability check, locks agreed pricing snapshots, and initiates payment escrow.
6. **`processEscrowPayment`** (`payment-feedback-service` / `POST /payments`, `POST /payouts`):
   - **Where in BookMe:** Located in `services/payment-feedback-service/src/modules/payment/`.
   - **Job:** Charges customer payment methods, deposits funds in escrow custody, and triggers vendor disbursements once services are fulfilled.
7. **`submitFeedbackAndDisputes`** (`payment-feedback-service` / `POST /reviews`, `POST /complaints`):
   - **Where in BookMe:** Located in `services/payment-feedback-service/src/modules/review/` & `complaint/`.
   - **Job:** Collects verified customer feedback to update vendor reputation scores and mediates dispute resolutions.

---

### 2. Service Boundaries & Data Ownership (Task 2)
As benchmarked in [`services.drawio`](../assignment-2/services.drawio) and [`services.png`](../assignment-2/services.png):
* **Auth Service (`auth-service`)** — Lead: Radhika Verma (`20251651075`):
  * **Owned Data:** `users`, `user_credentials`, `user_sessions`.
* **Vendor Service (`vendor-service`)** — Lead: Nandini Kanaujiya (`20251651061`):
  * **Owned Data:** `vendors`, `services`, `packages`, `availability`.
* **Event & Booking Service (`event-booking-service`)** — Lead: Alka Jha (`20251651013`):
  * **Owned Data:** `events`, `bookings`, `checklist_items`.
* **Payment & Feedback Service (`payment-feedback-service`)** — Lead: Alok Mishra (`20251651014`):
  * **Owned Data:** `payments`, `payouts`, `reviews`, `complaints`.

**Inter-Service Operations:**
* `event-booking-service` ➔ `auth-service`: `verifyCustomer(authToken)`
* `event-booking-service` ➔ `vendor-service`: `checkAvailability(vendorId, date)`
* `event-booking-service` ➔ `vendor-service`: `reserveSlot(vendorId, date, bookingRef)`
* `event-booking-service` ➔ `payment-feedback-service`: `initiatePayment(bookingId, amount)`
* `payment-feedback-service` ➔ `event-booking-service`: `updateBookingStatus(bookingId, status)`
* `payment-feedback-service` ➔ `vendor-service`: `getVendorPayoutAccount(vendorId)`

---

### 3. Central Operation: `createBooking` (Task 4)
* **REST Path:** `POST /api/v1/bookings`
* **Exposed by:** `event-booking-service`
* **Caller Request:**
  ```json
  {
    "eventId": "e9b28b21-4f08-48b6-b51c-6a17b0d91244",
    "vendorId": "vnd_sunset_caterers",
    "packageId": "pkg_royal_buffet",
    "date": "2026-12-15",
    "customizationNotes": "Vegetarian live counter required"
  }
  ```
* **Success Response (201 Created):**
  ```json
  {
    "bookingId": "bkg_7720914e-4b71-46ab",
    "eventId": "e9b28b21-4f08-48b6-b51c-6a17b0d91244",
    "status": "pending",
    "vendorNameSnapshot": "Sunset Royal Caterers & Event Crafters",
    "priceSnapshot": 45000.00,
    "date": "2026-12-15",
    "createdAt": "2026-10-13T18:00:00Z"
  }
  ```
* **Error Scenarios Handled:**
  * `400 Bad Request`: `INVALID_BOOKING_DATE` (Date in past or >18 months ahead)
  * `401 Unauthorized`: `UNAUTHENTICATED` (Token missing or expired)
  * `404 Not Found`: `EVENT_NOT_FOUND` / `PACKAGE_NOT_FOUND`
  * `409 Conflict`: `SLOT_UNAVAILABLE` (Vendor already booked or blocked on date)
  * `422 Unprocessable`: `BUDGET_EXCEEDED` (Cost exceeds event budget ceiling)
  * `503 Unavailable`: `VENDOR_SERVICE_TIMEOUT` (Downstream availability check timed out)
* **Implementation Encapsulation:**
  * **Hides Database Tables:** Callers are unaware that rows are inserted in `bookings`, aggregated in `events.budget`, and appended to `checklist_items`.
  * **Encapsulates Two-Phase Reservation:** Calls `vendor-service` to check and tentatively block the slot; rolls back if payment authorization fails.
  * **Price Snapshotting:** Rates are permanently frozen onto the booking record to insulate transactions from future vendor catalog edits.

---

### 4. Database Schema Benchmark (Task 5)
* **Boundary Rule:** No shared tables. No service queries another service's database directly.
* **Decoupled Attributes:** Logical UUIDs/strings are used instead of cross-database foreign keys (e.g., `bookings.vendor_id` logically points to `vendors.id`).
* Full DDL script is runnable in PostgreSQL: [`schema.sql`](../assignment-2/schema.sql).

---

### 5. Service Property Validation (Task 6)
All 4 microservices passed the 5 properties:
1. **Reachable:** RESTful endpoints exposed over defined HTTP ports and API Gateway routes.
2. **Self-Contained:** Each service owns its database and business logic without external database dependencies.
3. **Has a Contract:** Formal input/output schemas and RFC 7807 Problem Details contracts.
4. **Independent:** Autonomous development, deployment, and scalability.
5. **Loosely Coupled:** Decoupled via UUID identifiers. The latency coupling trade-off in synchronous availability checking is mitigated through short-lived caching and asynchronous Saga compensation workflows.
