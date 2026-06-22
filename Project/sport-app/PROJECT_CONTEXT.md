# 🧠 Project Context - sport-app
*Persistent workspace memory and technical references for the sport-app project*

## 🎯 Project Overview
* **Description:** A sport activity booking and management system.
* **Key Focus:** Booking lifecycle, scheduling, role-based access control, and clean user interfaces.

## ⚙️ Key Technical Solutions & Gotchas

### 1. Database Deletion Constraints
To protect financial transaction history, deletion of key booking foreign relations must be restricted.
* **Solution:** Apply restrictive constraints on foreign keys (e.g. `restrictOnDelete()`) in migration files to prevent accidental deletion of critical records linked to payments or bookings.

### 2. Unpaid Booking Expiration Scheduler (`bookings:expire`)
Unpaid booking slots must be freed up automatically after a certain expiration period to keep slot capacity available for other users.
* **Solution:** Created a console command `bookings:expire` scheduled to run periodically. The command transitions expired unpaid bookings to an `expired` status. Verified the behavior using feature tests.

### 3. Role-Based Access Control (RBAC) & DB Fallback
Integrating Spatie Laravel Permission for user roles.
* **Solution:** Set up role assignments with defensive checks. In cases where the Spatie role table or column checks fail, implement clean database level fallbacks or checks to verify permissions without breaking active user sessions.

## 🎨 UI Refactoring & Alignments
Refactored the core customer-facing views to look modern, professional, and balanced:
* **Activities Search:** Left-aligned grids, clean filter layout, premium card designs.
* **Booking Forms:** Clear instruction sections, dynamic fields, responsive layouts.
* **Bookings List:** Status badges, clear grouping (upcoming vs. past), quick actions.
* **Profile & Notifications:** Simplified forms, consistent spacing, intuitive layout.
