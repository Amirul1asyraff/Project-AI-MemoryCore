# Database Schema

## Entity Relationship Diagram (Conceptual)

```
┌──────────────┐         ┌──────────────┐
│    Users     │         │   Books      │
│ (Members &   │         │              │
│  Staff)      │         │ (20k items)  │
└──────┬───────┘         └──────┬───────┘
       │                        │
       │ creates                │
       │                        │
       └────────────────────────┼─────────────────┐
                                │                 │
                    ┌───────────┴────────┐    ┌────┴────────┐
                    │                    │    │             │
              ┌─────┴──────┐      ┌──────┴┐   │             │
              │ Borrowing  │      │Fines  │   │             │
              │ (Check-out │      │       │   │ Reservations│
              │  Records)  │      │       │   │             │
              └────────────┘      └───────┘   └─────────────┘
```

---

## Core Tables

### 1. `users` Table
User accounts for members and staff.

```sql
CREATE TABLE users (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    
    -- User Type (member, staff)
    user_type ENUM('member', 'staff') DEFAULT 'member',
    
    -- Member Type
    member_type ENUM('walk_in', 'registered', 'senior', 'student') 
        DEFAULT 'registered' COMMENT 'Only for members',
    
    -- Status
    status ENUM('active', 'suspended', 'inactive') DEFAULT 'active',
    suspension_reason VARCHAR(500),
    suspension_until DATETIME,
    
    -- Staff Role (if user_type = 'staff')
    staff_role ENUM('admin', 'librarian', 'front_desk'),
    
    -- Address
    address TEXT,
    postal_code VARCHAR(10),
    city VARCHAR(100),
    state VARCHAR(100),
    country VARCHAR(100) DEFAULT 'Malaysia',
    
    -- Emergency Contact
    emergency_contact_name VARCHAR(255),
    emergency_contact_phone VARCHAR(20),
    
    -- Membership
    membership_start_date DATE,
    membership_expiry_date DATE,
    
    -- Fine Balance
    total_fines_owed DECIMAL(10, 2) DEFAULT 0.00,
    
    -- Preferences
    language_preference ENUM('en', 'ms') DEFAULT 'en',
    receive_notifications BOOLEAN DEFAULT TRUE,
    receive_email_reminders BOOLEAN DEFAULT TRUE,
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deleted_at TIMESTAMP NULL COMMENT 'Soft delete'
);
```

### 2. `books` Table
Book catalog with inventory tracking.

```sql
CREATE TABLE books (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    isbn VARCHAR(13) UNIQUE,
    title VARCHAR(500) NOT NULL,
    author VARCHAR(255) NOT NULL,
    publisher VARCHAR(255),
    publication_year YEAR,
    
    -- Descriptions
    description TEXT,
    description_ms TEXT COMMENT 'Bahasa Malaysia description',
    
    -- Classification
    category VARCHAR(100),
    subcategory VARCHAR(100),
    dewey_decimal VARCHAR(20) COMMENT 'Dewey Decimal Classification',
    
    -- Physical Details
    isbn_barcode VARCHAR(13),
    call_number VARCHAR(50) COMMENT 'Library classification',
    language ENUM('en', 'ms', 'multilingual') DEFAULT 'en',
    
    -- Inventory
    total_copies INT DEFAULT 1,
    available_copies INT DEFAULT 1,
    reserved_copies INT DEFAULT 0,
    
    -- Borrowing Rules
    borrowing_period_days INT DEFAULT 21,
    renewal_limit INT DEFAULT 2,
    max_holds_per_member INT DEFAULT 1,
    
    -- Pricing
    replacement_cost DECIMAL(10, 2),
    
    -- Status
    status ENUM('active', 'archived', 'removed') DEFAULT 'active',
    
    -- Metadata
    tags VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    KEY idx_isbn (isbn),
    KEY idx_title (title),
    KEY idx_author (author),
    KEY idx_category (category),
    FULLTEXT INDEX ft_search (title, author, description)
);
```

### 3. `book_copies` Table
Individual physical copies of books.

```sql
CREATE TABLE book_copies (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    book_id BIGINT UNSIGNED NOT NULL,
    copy_number INT NOT NULL,
    barcode VARCHAR(50) UNIQUE COMMENT 'Physical barcode on book',
    
    -- Condition
    condition ENUM('excellent', 'good', 'fair', 'poor') DEFAULT 'good',
    last_condition_check DATETIME,
    
    -- Status
    status ENUM('available', 'borrowed', 'damaged', 'lost', 'maintenance') 
        DEFAULT 'available',
    
    acquisition_date DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (book_id) REFERENCES books(id) ON DELETE CASCADE,
    UNIQUE KEY uq_book_copy (book_id, copy_number),
    KEY idx_barcode (barcode),
    KEY idx_status (status)
);
```

### 4. `borrowings` Table
Record of all book borrowings and returns.

```sql
CREATE TABLE borrowings (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT UNSIGNED NOT NULL,
    book_copy_id BIGINT UNSIGNED NOT NULL,
    book_id BIGINT UNSIGNED NOT NULL,
    
    -- Dates
    borrowed_at DATETIME NOT NULL,
    due_date DATE NOT NULL,
    returned_at DATETIME,
    
    -- Renewal Info
    renewal_count INT DEFAULT 0,
    last_renewal_date DATETIME,
    
    -- Fine Information
    fine_amount DECIMAL(10, 2) DEFAULT 0.00,
    fine_paid BOOLEAN DEFAULT FALSE,
    fine_paid_at DATETIME,
    
    -- Status
    status ENUM('active', 'returned', 'lost', 'damaged') DEFAULT 'active',
    
    -- Extended Info
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (book_copy_id) REFERENCES book_copies(id),
    FOREIGN KEY (book_id) REFERENCES books(id),
    KEY idx_user (user_id),
    KEY idx_due_date (due_date),
    KEY idx_status (status),
    KEY idx_book (book_id)
);
```

### 5. `reservations` Table
Book reservation queue system.

```sql
CREATE TABLE reservations (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT UNSIGNED NOT NULL,
    book_id BIGINT UNSIGNED NOT NULL,
    
    -- Queue
    queue_position INT NOT NULL,
    
    -- Dates
    reserved_at DATETIME NOT NULL,
    expires_at DATETIME COMMENT '7-day hold period',
    picked_up_at DATETIME,
    cancelled_at DATETIME,
    
    -- Status
    status ENUM('waiting', 'ready', 'picked_up', 'expired', 'cancelled') 
        DEFAULT 'waiting',
    
    -- Notifications
    notification_sent_at DATETIME,
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (book_id) REFERENCES books(id),
    KEY idx_book_queue (book_id, queue_position),
    KEY idx_user_reserved (user_id),
    KEY idx_status (status)
);
```

### 6. `fines` Table
Detailed fine records.

```sql
CREATE TABLE fines (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT UNSIGNED NOT NULL,
    borrowing_id BIGINT UNSIGNED,
    
    -- Fine Details
    fine_amount DECIMAL(10, 2) NOT NULL,
    fine_reason ENUM('overdue', 'damage', 'lost_book', 'other') NOT NULL,
    description TEXT,
    
    -- Payment
    paid_amount DECIMAL(10, 2) DEFAULT 0.00,
    payment_date DATETIME,
    payment_method ENUM('cash', 'card', 'bank_transfer', 'waived') DEFAULT 'cash',
    
    -- Status
    status ENUM('unpaid', 'partial', 'paid', 'waived', 'disputed') DEFAULT 'unpaid',
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    FOREIGN KEY (user_id) REFERENCES users(id),
    FOREIGN KEY (borrowing_id) REFERENCES borrowings(id),
    KEY idx_user (user_id),
    KEY idx_status (status)
);
```

### 7. `notifications` Table
Email/notification history.

```sql
CREATE TABLE notifications (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT UNSIGNED NOT NULL,
    notification_type ENUM(
        'due_date_reminder',
        'overdue_notice',
        'reservation_ready',
        'fine_alert',
        'system_message'
    ) NOT NULL,
    
    -- Content
    title VARCHAR(255) NOT NULL,
    message TEXT NOT NULL,
    
    -- Language
    language ENUM('en', 'ms') DEFAULT 'en',
    
    -- Delivery
    delivery_method ENUM('email', 'sms', 'in_app') DEFAULT 'email',
    sent_at DATETIME,
    read_at DATETIME,
    
    -- Reference
    related_resource_id BIGINT UNSIGNED COMMENT 'book_id, borrowing_id, etc',
    related_resource_type VARCHAR(100),
    
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    FOREIGN KEY (user_id) REFERENCES users(id),
    KEY idx_user_type (user_id, notification_type),
    KEY idx_sent (sent_at)
);
```

### 8. `system_settings` Table
Configuration and system parameters.

```sql
CREATE TABLE system_settings (
    id BIGINT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
    setting_key VARCHAR(255) UNIQUE NOT NULL,
    setting_value TEXT,
    data_type ENUM('string', 'integer', 'boolean', 'json'),
    description VARCHAR(500),
    
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    KEY idx_key (setting_key)
);
```

---

## Key Settings (Configurable)

```json
{
  "default_borrowing_days": 21,
  "renewal_limit_per_book": 2,
  "fine_per_day_overdue": 1.00,
  "max_items_member": 5,
  "max_items_senior": 8,
  "max_items_student": 6,
  "reservation_hold_days": 7,
  "system_language": "en",
  "email_notification_enabled": true,
  "barcode_format": "ean13"
}
```

---

## Indexes for Performance

**Critical Indexes:**
- `borrowings.due_date` - For overdue reports
- `borrowings.user_id` - For member borrowing history
- `books.isbn` - For quick book lookup
- `book_copies.barcode` - For barcode scanner lookups
- `reservations.book_id, queue_position` - For reservation queue
- `fines.user_id, status` - For fine reports
- Full-text index on `books(title, author)` - For search

---

**Last Updated:** 2026-04-09
