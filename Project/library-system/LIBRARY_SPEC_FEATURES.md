# Features & Requirements

## Core Features Overview

```
Library Management System
├── Catalog Management
├── Book Borrowing & Returns
├── Reservation System
├── Fine Management
├── Member Management
├── Search & Discovery
├── Notifications
├── Reporting & Analytics
└── Admin Controls
```

---

## 1. Catalog Management

### 1.1 Book Addition & Editing
**User:** Admin, Librarian

```
Add Book:
- ISBN validation
- Title (English & Bahasa Malaysia)
- Author, Publisher, Publication year
- Category/Subcategory selection
- Description in both languages
- Book cover image upload
- Pricing/Replacement cost
- Set borrowing rules (default 21 days)
- Renewal limits (default 2)
```

**Requirements:**
- ✓ Duplicate ISBN detection
- ✓ Required fields validation
- ✓ Image optimization (auto-resize)
- ✓ Audit trail for changes

### 1.2 Physical Copy Management
**User:** Admin, Librarian

```
Book Copy Addition:
- Link to book master
- Barcode generation/assignment
- Condition tracking (Excellent, Good, Fair, Poor)
- Acquisition date
- Status assignment
```

**Requirements:**
- ✓ Generate barcodes (13-digit EAN)
- ✓ Print barcode labels
- ✓ Track copy number per book
- ✓ Update condition on periodic review

---

## 2. Book Borrowing & Returns

### 2.1 Checkout Process
**User:** Front Desk, Admin

```
Sequence:
1. Staff scans member barcode
2. System loads member profile
3. Validates member eligibility:
   - Account status (active)
   - Fine balance acceptable (< RM 50)
   - No excessive fines
4. Staff scans book barcode
5. System validates:
   - Book available
   - Member item count < limit
   - Book not restricted for member
6. Record created with due date
7. Confirmation displayed
8. Email receipt sent
9. Optional: Print physical receipt
```

**Requirements:**
- ✓ Real-time barcode scanning
- ✓ Instant validation feedback
- ✓ Visual/audio confirmation
- ✓ Support offline operation (queue sync)
- ✓ Transaction logging

### 2.2 Return Process
**User:** Front Desk, Admin

```
Sequence:
1. Staff scans book barcode
2. System identifies borrowing record
3. Calculate return status:
   - On-time return → Zero fine
   - Late return → Calculate fine (RM 1/day default)
4. Check book condition
5. Record return, mark copy as available
6. Display fine (if applicable)
7. Receipt/confirmation
8. Update inventory
9. Notification email
```

**Requirements:**
- ✓ Automatic fine calculation
- ✓ Option to record damage
- ✓ Option to record loss
- ✓ Receipt generation
- ✓ Email confirmation

### 2.3 Renewal System
**User:** Member (Self-service), Front Desk

```
Online Renewal:
1. Member views "Current Loans"
2. Clicks "Renew" on book
3. System checks:
   - Renewal limit not exceeded
   - Book not reserved by others
   - No overdue status
4. Extends due date (+21 days)
5. Confirmation displayed and emailed
```

**Requirements:**
- ✓ Check renewal limit per book
- ✓ Check for existing reservations
- ✓ Extend due date based on original
- ✓ Email confirmation
- ✓ Front desk can renew for members

---

## 3. Reservation System

### 3.1 Book Reservation
**User:** Member, Librarian (on behalf)

```
Reserve Book:
1. Member searches catalog
2. Book unavailable (all copies checked out)
3. Member clicks "Reserve"
4. System adds to queue
5. Queue position assigned
6. Confirmation sent
```

**Requirements:**
- ✓ Queue management
- ✓ Prevent duplicate reserves per member
- ✓ Max 1 reservation per member per book
- ✓ Queue priority (senior citizens first)
- ✓ Email notifications when available

### 3.2 Fulfillment Workflow
**User:** Librarian, Front Desk

```
When Book Returned:
1. Book marked as returned
2. System checks reservation queue
3. If reserved:
   - Status → "Ready for Pickup"
   - Email sent to member
   - 7-day hold period starts
4. Front desk receives notification
5. Physical book held at desk
```

**Requirements:**
- ✓ Automatic queue progression
- ✓ Notification generation
- ✓ 7-day hold period (configurable)
- ✓ Auto-cancel if not picked up
- ✓ Notify next in queue if expired

---

## 4. Fine Management

### 4.1 Overdue Fine Calculation
**Automatic** - Daily batch job

```
Daily Process:
1. Query all active borrowings
2. Check due date vs today
3. For each overdue item:
   - Days overdue = Today - Due date
   - Fine amount = Days × RM 1.00
   - Apply adjustments (senior 50% off)
   - Create fine record
4. Update user's fine balance
5. Generate overdue notices
```

**Requirements:**
- ✓ RM 1 per day (configurable)
- ✓ Senior citizen 50% discount
- ✓ Max fine per item: RM 50 (configurable)
- ✓ Automatic batch processing
- ✓ Grace period options (e.g., 1-day grace)

### 4.2 Fine Payment Recording
**User:** Front Desk, Member (partial online)

```
Record Payment:
1. Staff selects member
2. View outstanding fines
3. Select fines to pay
4. Enter payment amount
5. Select payment method:
   - Cash
   - Debit/Credit Card
   - Bank Transfer
6. Record payment
7. Issue receipt
8. Generate payment confirmation email
```

**Requirements:**
- ✓ Multiple payment methods
- ✓ Partial payment support
- ✓ Payment receipts (printable)
- ✓ Transaction logging
- ✓ No cash handling (optional POS integration)

### 4.3 Fine Waiver/Adjustment
**User:** Admin, Librarian (limited)

```
Waive Fine:
1. Admin/Librarian selects member
2. Views fine details
3. Reason for waiver:
   - System error
   - Staff error
   - Compassionate (with approval)
4. Enters waiver amount
5. Records reason
6. Marks as waived
7. Updates member balance
```

**Requirements:**
- ✓ Admin full authority
- ✓ Librarian limited (requires review)
- ✓ Audit trail mandatory
- ✓ Waiver email to member

---

## 5. Member Management

### 5.1 Member Registration
**User:** Front Desk, Member (Self-service online)

```
Walk-in Registration:
1. Collect information:
   - Full name, Phone, Email
   - Address, Postal code
   - City, State
   - Emergency contact
   - Member type selection
2. Photo ID verification
3. System generates member ID
4. Barcode generated and printed
5. Email confirmation sent
6. Welcome package provided
```

**Requirements:**
- ✓ Real-time duplicate check (email, phone)
- ✓ Email verification (optional)
- ✓ Member ID auto-generation
- ✓ Barcode printing
- ✓ Language preference selection

### 5.2 Member Profile Management
**User:** Member (self-service), Admin, Librarian

```
Member Can Update:
- Phone number
- Email address
- Address
- Password
- Language preference
- Notification preferences
```

**Staff Can Update:**
- Member type (walk-in → registered)
- Status (active, suspended, inactive)
- Suspension reason and end date
- Emergency contact

**Requirements:**
- ✓ Email change verification
- ✓ Password reset workflow
- ✓ Suspension notification
- ✓ Activity logging

### 5.3 Member Search
**User:** Front Desk, Librarian, Admin

```
Search Methods:
- Member ID (barcode scan or manual)
- Name (partial match)
- Email
- Phone number
- Card number
```

**Results Show:**
- Basic profile
- Current loans (due dates)
- Fine balance
- Reservation queue
- Membership status

**Requirements:**
- ✓ Case-insensitive search
- ✓ Partial matching
- ✓ Fast response (<1 second)
- ✓ Barcode scanner support

---

## 6. Search & Discovery

### 6.1 Book Search
**User:** All (including guests)

```
Search Options:
- Title
- Author
- ISBN
- Category/Subcategory
- Multiple filters combined
```

**Search Results Show:**
- Book cover, Title, Author
- Availability (X of Y copies available)
- Average rating (if enabled)
- Quick "Reserve" button (if unavailable)
- Quick "Borrow" button (if available, members only)
```

**Requirements:**
- ✓ Full-text search
- ✓ Autocomplete suggestions
- ✓ Filter by category
- ✓ Sort by title, author, newest, popularity
- ✓ Bilingual support
- ✓ Mobile optimized

### 6.2 Advanced Search
**User:** Members, Staff

```
Filters:
- Publication year range
- Language
- Availability status
- Member type eligibility
- New additions (last 30 days)
```

**Requirements:**
- ✓ Faceted search
- ✓ Save search preferences
- ✓ Email search results option

---

## 7. Notifications & Reminders

### 7.1 Automated Notifications

| Event | Recipient | Timing | Language |
|-------|-----------|--------|----------|
| Book Borrowed | Member | Immediately | Member preference |
| Due Date Reminder | Member | 3 days before | Member preference |
| Overdue Notice | Member | 1 day after due | Member preference |
| Reservation Ready | Member | When available | Member preference |
| Fine Alert | Member | When fine generated | Member preference |
| Return Confirmation | Member | Upon return | Member preference |

**Requirements:**
- ✓ Bilingual content (EN & MS)
- ✓ Member opt-in/opt-out
- ✓ Multiple reminders (optional escalation)
- ✓ Delivery method: Email (primary)
- ✓ SMS option (future)

### 7.2 Notification Templates
**Content:**
- Personalized greeting (member name)
- Book title and barcode
- Due/renewal dates
- Library contact info
- Account link (for self-service)

**Example:**
```
Subject: 📚 Book Due Reminder - [Book Title]

Dear [Member Name],

Your book "[Book Title]" is due on [Date] (3 days from now).

Renew online: [Link]
Library Phone: [Number]

Thank you!
```

---

## 8. Reporting & Analytics

### 8.1 Member Reports (Admin & Librarian)
- Active members (with stats)
- Overdue borrowers
- High-fine members
- Inactive members (no loans in 6 months)
- Member demographics

### 8.2 Inventory Reports (Admin & Librarian)
- Books by category
- Overdue books (not returned)
- Damaged/lost books
- High-demand books
- Underutilized collection
- Available vs checked-out counts

### 8.3 Financial Reports (Admin)
- Total fines collected
- Payment methods breakdown
- Outstanding fines
- Unpaid fines summary
- Monthly revenue trend

### 8.4 Circulation Reports (All staff)
- Daily circulation
- Popular books
- Borrowing trends
- Member activity
- Peak usage hours

**Requirements:**
- ✓ Exportable (PDF, CSV, Excel)
- ✓ Customizable date ranges
- ✓ Visual charts and graphs
- ✓ Scheduled email reports

---

## 9. System Administration

### 9.1 Configuration Settings
- Default borrowing period (days)
- Renewal limit per book
- Fine amount per day
- Senior citizen fine reduction %
- Max items per member type
- Reservation hold period (days)
- System language default

### 9.2 Staff Management (Admin only)
- Create staff accounts
- Assign roles (Librarian, Front Desk)
- View staff activity logs
- Suspend/deactivate accounts

### 9.3 System Health
- Database backup status
- Application logs
- Error tracking
- Performance monitoring
- User activity audit trail

**Requirements:**
- ✓ Configuration UI (no code changes)
- ✓ Backup scheduling
- ✓ Log retention (90 days)
- ✓ System status dashboard

---

**Last Updated:** 2026-04-09
