# System Architecture

## High-Level Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        Client Layer                              │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Web Browser / Mobile Browser (Responsive UI)            │   │
│  │  - Member Portal                                         │   │
│  │  - Staff Dashboard                                       │   │
│  │  - Admin Panel                                           │   │
│  └──────────────────────────────────────────────────────────┘   │
└──────────────────────┬──────────────────────────────────────────┘
                       │ HTTPS/REST API
┌──────────────────────┴──────────────────────────────────────────┐
│                     API Layer (Laravel)                          │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  API Routes & Controllers                                │   │
│  │  - Authentication                                        │   │
│  │  - Books (Catalog, Search)                               │   │
│  │  - Borrowing (Check-out, Check-in)                       │   │
│  │  - Members (Profile, History)                            │   │
│  │  - Reservations & Renewals                               │   │
│  │  - Fines Management                                      │   │
│  │  - Admin Functions                                       │   │
│  └──────────────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────────────┐   │
│  │  Middleware & Services                                   │   │
│  │  - Authentication/Authorization                          │   │
│  │  - Barcode Processing                                    │   │
│  │  - Email Notifications                                   │   │
│  │  - Localization (i18n)                                   │   │
│  │  - Fine Calculation                                      │   │
│  └──────────────────────────────────────────────────────────┘   │
└──────────────────────┬──────────────────────────────────────────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
┌───────┴──────┐  ┌────┴─────┐  ┌────┴─────────┐
│  Database    │  │  File    │  │  Email       │
│  (MySQL/     │  │  Storage │  │  Service     │
│   PostgreSQL)│  │  (Local) │  │  (SMTP)      │
└──────────────┘  └──────────┘  └──────────────┘
```

---

## System Components

### 1. **Frontend (Client Layer)**
- **Technology:** Vue.js 3 SPA with Vite build tool
- **State Management:** Pinia for centralized state
- **Routing:** Vue Router for client-side navigation
- **Styling:** Tailwind CSS with responsive design
- **API Communication:** Axios with Laravel Sanctum authentication
- **Languages:** English & Bahasa Malaysia with Vue i18n
- **Performance:** Code splitting, lazy loading, optimized for slow connections

### 2. **Backend (Laravel Application)**

#### API Layer
- **Framework:** Laravel 11 (latest stable)
- **API Style:** RESTful with JSON responses
- **Authentication:** Laravel Sanctum (SPA + API tokens)
- **Authorization:** Spatie Laravel Permission (RBAC)
- **Rate Limiting:** Prevent abuse, optimize barcode scanner requests
- **CORS:** Sanctum stateful middleware for SPA

#### Core Services
- **AuthService** - User authentication and token management
- **BookService** - Book catalog operations
- **BorrowingService** - Handle check-out/check-in logic
- **FineService** - Calculate and track fines
- **NotificationService** - Email/SMS alerts
- **BarcodeService** - Process barcode input

#### Controllers
- `AuthController` - Login, logout, registration
- `BookController` - Search, browse catalog
- `BorrowController` - Check-out, check-in, renewals
- `MemberController` - Profile, borrowing history
- `ReservationController` - Book reservations
- `AdminController` - System management
- `ReportController` - Librarian reports

### 3. **Database Layer**
- **DBMS:** MySQL 8.0+ or PostgreSQL 13+
- **Schema:** Normalized relational design
- **Backup:** Daily automated backups
- **Replication:** Optional for high availability

### 4. **External Integrations**

#### Barcode Scanner
- USB HID barcode scanner (keyboard input simulation)
- Reads 13-digit ISBN and member ID barcodes
- Real-time validation and processing

#### Email Service
- SMTP server (Gmail, SendGrid, or local)
- Notification templates (bilingual)
- Queue system for bulk notifications

---

## Data Flow Examples

### Borrowing Flow
```
1. Member arrives at library
2. Front desk staff opens web app
3. Scans member barcode → System loads member profile
4. Scans book barcode → System loads book details
5. System validates:
   - Member eligibility (active, no excessive fines)
   - Book availability
   - Member's current items count
6. Creates borrowing record with due date
7. Email confirmation sent
8. Receipt printed/displayed
```

### Return Flow
```
1. Member at counter with book
2. Staff scans book barcode
3. System checks if book is on loan
4. Marks as returned
5. Checks for overdue:
   - If late → Calculate fine
   - If on-time → Thank you message
6. Updates inventory
7. Notification email sent
```

---

## Scalability Considerations

### For ~20,000 Books & 2,000 Members

**Phase 1 (Current):**
- Single server deployment
- MySQL database
- Local file storage
- Suitable for initial launch

**Phase 2 (Future Expansion):**
- Load balancing if concurrent users exceed 100
- Database read replicas for reporting
- Redis caching for catalog searches
- CDN for static assets

---

## Security Architecture

### Authentication & Authorization
- **Framework:** Laravel's built-in authentication
- **Passwords:** Bcrypt hashing (Laravel default)
- **Sessions:** Secure HTTP-only cookies
- **API:** JWT tokens with expiration
- **Role-based Access Control (RBAC):** Admin, Librarian, Front Desk, Member

### Data Protection
- **HTTPS:** Encrypted all communications
- **Database:** Parameterized queries (prevents SQL injection)
- **Input Validation:** Server-side validation on all inputs
- **CORS:** Configured for same-origin requests only

---

## Deployment Architecture

```
Development Environment
  ↓
Staging Environment (Testing, UAT)
  ↓
Production Environment
  - Server: Linux (Ubuntu 20.04+)
  - Web Server: Nginx
  - PHP: PHP 8.2+
  - Database: Managed MySQL instance
  - Storage: Secure file storage
  - Monitoring: Error logging, performance monitoring
```

---

## Technology Decision Rationale

| Component | Choice | Why |
|-----------|--------|-----|
| **Framework** | Laravel 11 | Strong ecosystem, excellent docs, rapid dev |
| **Database** | MySQL | Reliable, proven, easy to backup/restore |
| **Frontend** | Blade + Alpine.js | Lightweight, simple, Laravel-integrated |
| **Authentication** | Laravel Breeze | Built-in, secure, minimal overhead |
| **Notifications** | Queue system | Reliable, non-blocking |

---

**Last Updated:** 2026-04-09
