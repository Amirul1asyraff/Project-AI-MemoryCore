# User Roles & Permissions

## Role Hierarchy

```
┌─────────────────────────────────────────┐
│            Admin                        │
│  (Full system access)                   │
└──────────────┬──────────────────────────┘
               │
               ├──────────────────────────┐
               │                          │
        ┌──────┴──────┐          ┌────────┴─────────┐
        │  Librarian  │          │  Front Desk      │
        │             │          │  (Limited staff) │
        └──────┬──────┘          └────────┬─────────┘
               │                          │
               └──────────────┬───────────┘
                              │
                    ┌─────────┴──────────┐
                    │                    │
              ┌─────┴────────┐    ┌──────┴──────────┐
              │ Registered   │    │ Walk-in / Guest │
              │ Members      │    │ (No account)    │
              └──────────────┘    └─────────────────┘
```

---

## Role Definitions

### 1. Admin
**Type:** Staff  
**System Access:** Full  
**Purpose:** System administration and management

#### Permissions

| Feature | Permission | Notes |
|---------|-----------|-------|
| **User Management** | Create, Edit, Delete, Suspend users | Can manage all user types |
| **Book Management** | Add, Edit, Delete books | Full catalog control |
| **Inventory** | View, Update stock | Check book conditions |
| **Fines** | Waive, Modify, Delete | Discretionary power |
| **Reports** | Generate all reports | Member, financial, inventory |
| **System Settings** | Configure all settings | Fine rates, borrowing periods |
| **Staff Management** | Create/manage staff accounts | Assign roles |
| **Backup & Restore** | Data management | System backups |
| **Analytics** | View all dashboards | Usage statistics |
| **Audit Logs** | View system logs | Track all actions |

#### Dashboard Elements
- System overview (users, books, active loans)
- Member management panel
- Financial reports (fines collected)
- Inventory management
- System health monitoring
- Staff activity logs

---

### 2. Librarian
**Type:** Staff  
**System Access:** Moderate  
**Purpose:** Cataloging, inventory, and library operations

#### Permissions

| Feature | Permission | Notes |
|---------|-----------|-------|
| **Book Management** | Create, Edit, View books | Cannot delete |
| **Inventory** | Check stock, Update conditions | View all copies |
| **Member Profiles** | View, Update (limited) | Cannot delete |
| **Borrowing** | View borrowing history | Reports only |
| **Fines** | View, Generate notices | Cannot modify/waive |
| **Reservations** | Manage queue | Fulfill reservations |
| **Reports** | Generate custom reports | Member, inventory reports |
| **Notifications** | Create, Send messages | Bulk communications |
| **Analytics** | View library statistics | Usage patterns |

#### Dashboard Elements
- Book cataloging section
- Inventory management
- Member search
- Borrowing reports
- Reservation queue
- Overdue notices
- Custom reporting tools

---

### 3. Front Desk
**Type:** Staff  
**System Access:** Limited  
**Purpose:** Daily check-in/check-out operations

#### Permissions

| Feature | Permission | Notes |
|---------|-----------|-------|
| **Book Checkout** | Check out books (barcode) | Primary function |
| **Book Return** | Check in books (barcode) | Calculate fines |
| **Renewals** | Renew books for members | Subject to limits |
| **Member Search** | Search members by ID/name | View basic info only |
| **Fine Payment** | Record fine payments | Cash/card/bank transfers |
| **Reservations** | Fulfill ready reservations | Mark picked up |
| **Quick Reports** | View daily stats | Circulation, fines |
| **Member Registration** | Register new members | Basic info entry |

#### Dashboard Elements
- Quick checkout/check-in panel
- Member search (quick access)
- Fine payment recording
- Daily circulation report
- Quick member lookup by barcode

---

### 4. Registered Member
**Type:** Member  
**System Access:** Self-service  
**Purpose:** Borrow books and manage account

#### Permissions

| Feature | Permission | Notes |
|---------|-----------|-------|
| **Catalog Search** | Browse and search books | View descriptions |
| **Book Details** | View availability, reviews | See copy locations |
| **Borrowing** | Check borrowed items | 5 items max (default) |
| **Renewals** | Renew books (online) | 2x per item limit |
| **Reservations** | Reserve unavailable books | 1 hold at a time |
| **Account** | View profile, Update password | Change contact info |
| **Borrowing History** | View past loans | Dates and fines |
| **Fines** | View current fines | Online payment option |
| **Notifications** | Manage preferences | Email reminders |

#### Member Portal Features
- Search and browse catalog
- View current loans (due dates)
- Renew books online
- Reserve books
- View fine balance
- Download receipts
- Update profile

---

### 5. Senior Citizen Member
**Type:** Member (Special privileges)  
**System Access:** Self-service + Benefits  
**Purpose:** Library access with senior benefits

#### Special Privileges

| Privilege | Details | Notes |
|-----------|---------|-------|
| **Borrow Limit** | 8 items (vs 5) | Higher allowance |
| **Fine Rate** | 50% reduction | Discounted fines |
| **Renewal Limit** | 3x per item (vs 2x) | Extended renewals |
| **Priority Hold** | Reserved items held 10 days (vs 7) | Extra time for pickup |
| **Reservation Priority** | Queue priority | Ahead of standard members |

#### Same Permissions As
- Registered Members (all features)
- Plus senior-specific benefits

---

### 6. Student Member
**Type:** Member (Educational)  
**System Access:** Self-service  
**Purpose:** Educational library access

#### Special Rules

| Rule | Details | Notes |
|------|---------|-------|
| **Borrow Limit** | 6 items | Academic standard |
| **Fine Rate** | Standard | No reduction |
| **Renewal Limit** | 2x per item | Same as registered |
| **Eligible Books** | All except restricted | May exclude reference-only |
| **Membership Validity** | Semester-based | Annual renewal |

#### Same Permissions As
- Registered Members (all features)

---

### 7. Walk-in / Guest (Unregistered)
**Type:** Public access  
**System Access:** Read-only  
**Purpose:** Browse library catalog

#### Permissions

| Feature | Permission | Notes |
|---------|-----------|-------|
| **Catalog Search** | Browse and search | No account needed |
| **Book Details** | View information | Availability only |
| **Contact Info** | View hours, contact | Self-service info |
| **Notifications** | None | Email not applicable |

#### Limitations
- Cannot borrow books
- Cannot reserve
- Cannot view fines
- No personal account

---

## Permission Matrix

```
                    Admin  Librarian  Front Desk  Member  Guest
                    ──────────────────────────────────────────
Add/Edit Books       ✓       ✓          ✗          ✗       ✗
Delete Books         ✓       ✗          ✗          ✗       ✗
Check Out Books      ✓       ✗          ✓          ✗       ✗
Check In Books       ✓       ✗          ✓          ✗       ✗
Renew Books          ✓       ✗          ✓          ✓       ✗
Reserve Books        ✓       ✗          ✗          ✓       ✗
View Fines           ✓       ✓          ✓          ✓       ✗
Modify Fines         ✓       ✗          ✗          ✗       ✗
Manage Members       ✓       ✓          ✗          ✗       ✗
Generate Reports     ✓       ✓          ✓          ✗       ✗
System Settings      ✓       ✗          ✗          ✗       ✗
Search Catalog       ✓       ✓          ✓          ✓       ✓
```

---

## Authentication & Authorization Implementation

### Session Management
```
1. User logs in with username/password
2. System validates credentials
3. JWT token generated with role information
4. Token stored in secure HTTP-only cookie
5. Token expires after 8 hours (configurable)
6. Refresh token for extended sessions
```

### Authorization Checks
```
Every API request:
1. Verify JWT token validity
2. Check user status (active/suspended)
3. Validate requested action against role
4. Log access attempt
5. Return 403 Forbidden if unauthorized
```

### Role-Based Route Protection
```
Example: /admin/settings
- Only accessible to Admin role
- Other roles get 403 response

Example: /api/books/{id}/checkout
- Accessible to: Admin, Front Desk
- Others get 403 response
```

---

## Data Visibility Rules

### Member Can See
- Own borrowing history
- Own fines and payments
- Own reservations
- Own profile information

### Member Cannot See
- Other members' information
- Staff-only reports
- System settings
- Financial data (except own fines)

### Librarian Can See
- All member profiles
- All borrowing records
- Inventory details
- Overdue reports
- Usage statistics

### Admin Can See
- Everything (full access)
- Audit logs
- System configuration
- Staff activity
- Financial summaries

---

**Last Updated:** 2026-04-09
