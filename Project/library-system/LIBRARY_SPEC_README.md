# Library Management System - Complete Specification

## Project Overview

**Project Name:** Malaysia Public Library Management System (PLMS)

**Version:** 1.0.0

**Location:** Malaysia

**Languages:** English & Bahasa Malaysia (Bilingual)

---

## Quick Facts

| Aspect | Details |
|--------|---------|
| **Book Inventory** | ~20,000 books |
| **Registered Members** | 2,000+ members |
| **Member Types** | Walk-in public, Registered, Senior citizens, Students |
| **Staff Roles** | Admin, Librarian, Front Desk |
| **Key Features** | Borrowing, Returns, Renewals, Reservations, Fines, Search |
| **Integrations** | Barcode scanner, Email notifications |
| **Platform** | Web (Mobile responsive) |
| **Framework** | Laravel 11 |

---

## Documentation Structure

This specification is organized into the following documents:

1. **[System Architecture](./LIBRARY_SPEC_ARCHITECTURE.md)** - Technical design and system components
2. **[Database Schema](./LIBRARY_SPEC_DATABASE.md)** - Complete data model and relationships
3. **[User Roles & Permissions](./LIBRARY_SPEC_ROLES.md)** - Access control and user types
4. **[Features & Requirements](./LIBRARY_SPEC_FEATURES.md)** - Detailed functional requirements
5. **[API Endpoints](./LIBRARY_SPEC_API.md)** - RESTful API specifications
6. **[UI/UX Guidelines](./LIBRARY_SPEC_UI.md)** - Interface design principles
7. **[Technology Stack](./LIBRARY_SPEC_TECH.md)** - Tool and package selections
8. **[Implementation Roadmap](./LIBRARY_SPEC_ROADMAP.md)** - Development phases and timeline
9. **[Security Considerations](./LIBRARY_SPEC_SECURITY.md)** - Security requirements and measures

---

## Core Objectives

✓ Easy-to-use interface for non-tech-savvy users  
✓ Support for bilingual content (English + Bahasa Malaysia)  
✓ Efficient book borrowing and return process  
✓ Automated fine calculation and member notifications  
✓ Mobile-responsive design for on-the-go access  
✓ Barcode integration for quick check-in/out  
✓ Member self-service capabilities  

---

## Member Types & Capabilities

| Member Type | Capabilities | Notes |
|------------|--------------|-------|
| **Walk-in Public** | View catalog, Browse books | No borrowing rights |
| **Registered Members** | Borrow (5 items), Return, Renew, Reserve, View fines | Standard members |
| **Senior Citizens** | Borrow (8 items), Reduced fines, Priority reservations | Special privileges |
| **Students** | Borrow (6 items), Return, Renew, Reserve | Educational access |

---

## Key System Workflows

### Borrowing Process
1. Member selects book from catalog
2. Frontend desk scans member barcode + book barcode
3. System checks member eligibility (fines, item limit)
4. Record created, due date set (21 days default)
5. Confirmation email sent

### Return Process
1. Member returns book to front desk
2. Barcode scanned
3. System marks as returned, calculates overdue fines if applicable
4. Receipt generated
5. Email confirmation sent

### Reservation System
1. Book unavailable? Member can reserve
2. System tracks queue
3. Email notification when book available
4. 7-day hold period for pickup

---

## Next Steps

1. Review this specification thoroughly
2. Confirm requirements and scope
3. Begin database schema creation
4. Set up Laravel project structure
5. Implement authentication system
6. Build core features (borrowing/returns)
7. Integrate barcode scanner
8. Deploy and test

---

**Last Updated:** 2026-04-09  
**Status:** Ready for Development
