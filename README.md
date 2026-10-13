# BookMe — Web Services Assignments Repository (CS 543)

[![Course](https://img.shields.io/badge/Course-CS%20543%20Web%20Services-blue.svg)]()
[![Platform](https://img.shields.io/badge/Project-BookMe%20(Team%2011)-brightgreen.svg)]()
[![Repository](https://img.shields.io/badge/GitHub-BookMe__Assignment-orange.svg)](https://github.com/Nandinikanaujiya/BookMe_Assignment)

Welcome to the official repository for the **CS 543 Web Services** assignments implemented with respect to the semester course project: **BookMe — AI-Powered Multi-Vendor Event Service Booking Platform**.

---

### 👥 Team 11 Members & Service Ownership

| Member Name | University Roll No. | Assigned Service | Service Responsibilities & Data Owned |
| :--- | :--- | :--- | :--- |
| **Radhika Verma** | `20251651075` | **Auth Service** (`auth-service`) | Registration, JWT auth, RBAC (`users`, `user_credentials`, `user_sessions`) |
| **Nandini Kanaujiya** | `20251651061` | **Vendor Service** (`vendor-service`) | Profiles, catalogs, packages, calendar availability (`vendors`, `services`, `packages`, `availability`) |
| **Alka Jha** | `20251651013` | **Event & Booking Service** (`event-booking-service`) *(Central)* | Event containers, checklists, booking lifecycle (`events`, `bookings`, `checklist_items`) |
| **Alok Mishra** | `20251651014` | **Payment & Feedback Service** (`payment-feedback-service`) | Escrow transactions, vendor payouts, reviews, complaints (`payments`, `payouts`, `reviews`, `complaints`) |

---

## 🔗 Submission & Working Demonstration Links

* **GitHub Repository:** [https://github.com/Nandinikanaujiya/BookMe_Assignment](https://github.com/Nandinikanaujiya/BookMe_Assignment)
* **Working Demonstration (Google Drive):** [https://drive.google.com/drive/folders/YOUR_DEMO_FOLDER_LINK?usp=sharing](https://drive.google.com/drive/folders/YOUR_DEMO_FOLDER_LINK?usp=sharing)  
  *(Share setting: Configured with "anyone with the link" permissions for grading access)*

---

## 📂 Quick Links to Assignments

### 📝 [Assignment 1 — HTTP by Hand & Project Setup](bookme/docs/assignment-1.md)
* **Project Architectural Brief:** [`bookme/brief.md`](bookme/brief.md) (Identified Nouns & Verbs)
* **HTTP Diagnostics Log:** [`bookme/http-log.md`](bookme/http-log.md) (Raw `curl -i` requests and header analysis)
* **Browser DevTools Profiling:** [`bookme/network-analysis.md`](bookme/network-analysis.md)
* **Assignment Summary:** [`bookme/docs/assignment-1.md`](bookme/docs/assignment-1.md)

### 🚀 [Assignment 2 — Design BookMe: Services, Contracts & Schema](bookme/docs/assignment-2.md)
* **Design Benchmark Document:** [`bookme/assignment-2/design.pdf`](bookme/assignment-2/design.pdf) (Capabilities, Contracts, `createBooking` Spec & Validation Table)
* **Service Architecture Benchmark Diagram:** [`bookme/assignment-2/services.drawio`](bookme/assignment-2/services.drawio) (Editable Draw.io) / [`bookme/assignment-2/services.png`](bookme/assignment-2/services.png) (Exported Image)
* **Database Schema ERD Benchmark:** [`bookme/assignment-2/schema.drawio`](bookme/assignment-2/schema.drawio) (Editable Draw.io) / [`bookme/assignment-2/schema.png`](bookme/assignment-2/schema.png) (Exported Image)
* **Database DDL:** [`bookme/assignment-2/schema.sql`](bookme/assignment-2/schema.sql) (PostgreSQL schema with strict boundary isolation)
* **Submission Brief PDF:** [`ReadMe.pdf`](ReadMe.pdf) / [`bookme/assignment-2/ReadMe.pdf`](bookme/assignment-2/ReadMe.pdf)
* **Assignment 2 Codebase Guide:** [`bookme/docs/assignment-2.md`](bookme/docs/assignment-2.md)
