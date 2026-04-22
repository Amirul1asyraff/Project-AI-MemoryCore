# API Endpoints

## API Overview

**Base URL:** `https://library.example.com/api`  
**Version:** v1  
**Authentication:** JWT Bearer Token  
**Response Format:** JSON  
**Rate Limiting:** 100 requests/minute per user

---

## Authentication Endpoints

### POST `/auth/login`
User login with credentials.

```json
Request:
{
  "username_or_email": "amirul@example.com",
  "password": "secure_password"
}

Response (200):
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": 1,
    "name": "Amirul",
    "email": "amirul@example.com",
    "user_type": "member",
    "member_type": "registered",
    "role": "member"
  }
}
```

### POST `/auth/logout`
Invalidate current session.

```json
Headers: Authorization: Bearer {token}

Response (200):
{
  "success": true,
  "message": "Logged out successfully"
}
```

### POST `/auth/register`
Register new member (walk-in or online).

```json
Request:
{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "0123456789",
  "password": "secure_password",
  "member_type": "registered",
  "address": "123 Main St",
  "postal_code": "50000",
  "city": "Kuala Lumpur",
  "state": "Selangor",
  "language_preference": "en"
}

Response (201):
{
  "success": true,
  "member_id": "LM000123",
  "message": "Registration successful. Verification email sent."
}
```

### POST `/auth/refresh`
Refresh JWT token.

```json
Request:
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIs..."
}

Response (200):
{
  "token": "new_jwt_token_here"
}
```

---

## Book Endpoints

### GET `/books`
Search and browse books.

```
Query Parameters:
  ?search=php                    // Title/Author search
  ?category=technology           // Filter by category
  ?availability=available        // available, reserved, all
  ?sort=newest                   // newest, popular, title
  ?page=1&limit=20              // Pagination
  ?language=en                   // Filter by language

Response (200):
{
  "success": true,
  "data": [
    {
      "id": 1,
      "isbn": "9781491927282",
      "title": "Learning PHP",
      "title_ms": "Belajar PHP",
      "author": "David Sklar",
      "category": "Technology",
      "available_copies": 2,
      "total_copies": 3,
      "cover_image_url": "https://...",
      "description": "..."
    }
  ],
  "pagination": {
    "total": 250,
    "page": 1,
    "limit": 20,
    "pages": 13
  }
}
```

### GET `/books/:id`
Get detailed book information.

```json
Response (200):
{
  "success": true,
  "data": {
    "id": 1,
    "isbn": "9781491927282",
    "title": "Learning PHP",
    "author": "David Sklar",
    "publisher": "O'Reilly",
    "publication_year": 2016,
    "description": "...",
    "category": "Technology",
    "available_copies": 2,
    "total_copies": 3,
    "borrowing_period_days": 21,
    "renewal_limit": 2,
    "copies": [
      {
        "id": 1,
        "copy_number": 1,
        "barcode": "9781491927282",
        "condition": "good",
        "status": "available"
      },
      {
        "id": 2,
        "copy_number": 2,
        "status": "borrowed",
        "due_date": "2026-04-25"
      }
    ],
    "reviews": [
      {
        "member_id": 5,
        "rating": 5,
        "comment": "Great book!",
        "date": "2026-03-15"
      }
    ]
  }
}
```

### POST `/books` (Admin/Librarian only)
Add new book to catalog.

```json
Request:
{
  "isbn": "9781491927282",
  "title": "Learning PHP",
  "title_ms": "Belajar PHP",
  "author": "David Sklar",
  "publisher": "O'Reilly",
  "publication_year": 2016,
  "description": "...",
  "description_ms": "...",
  "category": "Technology",
  "language": "en",
  "borrowing_period_days": 21,
  "renewal_limit": 2,
  "replacement_cost": 89.99
}

Response (201):
{
  "success": true,
  "book_id": 1,
  "message": "Book added successfully"
}
```

---

## Book Copy Endpoints

### POST `/books/:id/copies` (Admin/Librarian only)
Add physical copy.

```json
Request:
{
  "barcode": "9781491927282-001",
  "condition": "good"
}

Response (201):
{
  "success": true,
  "copy_id": 45,
  "copy_number": 1,
  "barcode": "9781491927282-001"
}
```

### PUT `/book-copies/:id` (Admin/Librarian only)
Update copy condition/status.

```json
Request:
{
  "condition": "fair",
  "status": "maintenance",
  "notes": "Water damage on spine"
}

Response (200):
{
  "success": true,
  "message": "Copy updated"
}
```

---

## Borrowing Endpoints

### POST `/borrowings` (Front Desk/Admin only)
Check out a book.

```json
Request:
{
  "member_id": 1,
  "book_copy_id": 45
}

Response (201):
{
  "success": true,
  "borrowing": {
    "id": 123,
    "book_title": "Learning PHP",
    "member_name": "John Doe",
    "borrowed_at": "2026-04-09T10:30:00Z",
    "due_date": "2026-04-30",
    "days_available": 21
  }
}
```

### GET `/borrowings/active` (Member)
Get current active loans.

```json
Response (200):
{
  "success": true,
  "data": [
    {
      "id": 123,
      "book_id": 1,
      "book_title": "Learning PHP",
      "book_cover": "https://...",
      "borrowed_at": "2026-04-09T10:30:00Z",
      "due_date": "2026-04-30",
      "days_remaining": 21,
      "is_overdue": false,
      "renewable": true,
      "renewal_count": 0
    }
  ]
}
```

### POST `/borrowings/:id/return` (Front Desk/Admin only)
Return a book.

```json
Request:
{
  "condition": "good",
  "notes": "Returned in good condition"
}

Response (200):
{
  "borrowing": {
    "id": 123,
    "book_title": "Learning PHP",
    "returned_at": "2026-04-20T14:15:00Z",
    "days_overdue": 0,
    "fine_amount": 0
  },
  "message": "Book returned successfully"
}
```

### POST `/borrowings/:id/renew` (Member/Front Desk)
Renew a book.

```json
Response (200):
{
  "success": true,
  "borrowing": {
    "id": 123,
    "new_due_date": "2026-05-21",
    "renewals_remaining": 1
  },
  "message": "Book renewed successfully"
}
```

### GET `/borrowings/history` (Member)
View borrowing history.

```json
Response (200):
{
  "success": true,
  "data": [
    {
      "id": 122,
      "book_title": "Laravel Guide",
      "borrowed_at": "2026-03-15",
      "returned_at": "2026-04-08",
      "days_overdue": 2,
      "fine_paid": true
    }
  ],
  "pagination": { ... }
}
```

---

## Reservation Endpoints

### POST `/reservations` (Member)
Reserve a book.

```json
Request:
{
  "book_id": 1
}

Response (201):
{
  "success": true,
  "reservation": {
    "id": 45,
    "book_title": "Learning PHP",
    "queue_position": 3,
    "reserved_at": "2026-04-09T11:00:00Z"
  },
  "message": "Book reserved. Notification will be sent when available."
}
```

### GET `/reservations` (Member)
View active reservations.

```json
Response (200):
{
  "success": true,
  "data": [
    {
      "id": 45,
      "book_title": "Learning PHP",
      "queue_position": 3,
      "status": "waiting",
      "reserved_at": "2026-04-09"
    }
  ]
}
```

### DELETE `/reservations/:id` (Member)
Cancel reservation.

```json
Response (200):
{
  "success": true,
  "message": "Reservation cancelled"
}
```

### POST `/reservations/:id/fulfill` (Librarian only)
Mark reservation as ready for pickup.

```json
Response (200):
{
  "success": true,
  "message": "Reservation marked as ready. Email notification sent."
}
```

---

## Fine Endpoints

### GET `/fines` (Member)
View personal fines.

```json
Response (200):
{
  "success": true,
  "total_outstanding": 15.50,
  "data": [
    {
      "id": 1,
      "book_title": "Laravel Guide",
      "fine_amount": 10.00,
      "fine_reason": "overdue",
      "days_overdue": 10,
      "status": "unpaid",
      "created_at": "2026-04-01"
    },
    {
      "id": 2,
      "book_title": "PHP Basics",
      "fine_amount": 5.50,
      "fine_reason": "overdue",
      "status": "unpaid",
      "created_at": "2026-04-05"
    }
  ]
}
```

### GET `/fines?user_id=:id` (Admin/Librarian)
View member's fines.

### POST `/fines/:id/pay` (Member/Front Desk)
Record fine payment.

```json
Request:
{
  "amount": 15.50,
  "payment_method": "cash"
}

Response (200):
{
  "success": true,
  "fine": {
    "id": 1,
    "status": "paid",
    "paid_amount": 15.50,
    "receipt_number": "RCP-2026-000145"
  }
}
```

### PUT `/fines/:id/waive` (Admin only)
Waive a fine.

```json
Request:
{
  "reason": "Staff error",
  "notes": "System error recorded due date incorrectly"
}

Response (200):
{
  "success": true,
  "message": "Fine waived"
}
```

---

## Member Endpoints

### GET `/members/:id` (Member, Admin, Librarian)
Get member profile.

```json
Response (200):
{
  "success": true,
  "data": {
    "id": 1,
    "member_id": "LM000001",
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "0123456789",
    "member_type": "registered",
    "status": "active",
    "membership_start_date": "2024-01-15",
    "total_items_borrowed": 5,
    "total_fines_owed": 15.50,
    "language_preference": "en"
  }
}
```

### PUT `/members/:id` (Member, Admin)
Update member profile.

```json
Request:
{
  "phone": "0987654321",
  "address": "456 New St",
  "language_preference": "ms",
  "receive_notifications": true
}

Response (200):
{
  "success": true,
  "message": "Profile updated"
}
```

### PUT `/members/:id/suspend` (Admin only)
Suspend member account.

```json
Request:
{
  "reason": "Excessive overdue books",
  "until_date": "2026-05-09"
}

Response (200):
{
  "success": true,
  "message": "Member suspended until 2026-05-09"
}
```

---

## Admin/Librarian Endpoints

### GET `/admin/reports/circulation`
Daily circulation report.

```json
Query Parameters:
  ?start_date=2026-04-01
  ?end_date=2026-04-09

Response (200):
{
  "success": true,
  "date_range": "2026-04-01 to 2026-04-09",
  "checkouts": 145,
  "returns": 142,
  "renewals": 23,
  "new_members": 12,
  "fines_collected": 234.50,
  "top_books": [ ... ]
}
```

### GET `/admin/reports/overdue`
Overdue books report.

```json
Response (200):
{
  "success": true,
  "overdue_count": 12,
  "data": [
    {
      "borrowing_id": 123,
      "member_name": "Jane Smith",
      "book_title": "Laravel Guide",
      "due_date": "2026-03-25",
      "days_overdue": 15,
      "fine_generated": 15.00,
      "member_email": "jane@example.com"
    }
  ]
}
```

### GET `/admin/reports/fines`
Financial summary.

```json
Response (200):
{
  "success": true,
  "period": "2026-04",
  "total_fines_created": 450.00,
  "total_fines_paid": 380.00,
  "outstanding_fines": 70.00,
  "fines_waived": 50.00
}
```

---

## Error Responses

### 400 Bad Request
```json
{
  "success": false,
  "error": "Invalid input",
  "message": "Email is required"
}
```

### 401 Unauthorized
```json
{
  "success": false,
  "error": "Unauthorized",
  "message": "Invalid token or not authenticated"
}
```

### 403 Forbidden
```json
{
  "success": false,
  "error": "Forbidden",
  "message": "You don't have permission for this action"
}
```

### 404 Not Found
```json
{
  "success": false,
  "error": "Not found",
  "message": "Book with ID 999 not found"
}
```

### 429 Too Many Requests
```json
{
  "success": false,
  "error": "Rate limit exceeded",
  "message": "Too many requests. Try again in 60 seconds"
}
```

### 500 Internal Server Error
```json
{
  "success": false,
  "error": "Server error",
  "message": "An unexpected error occurred"
}
```

---

## Webhook Events (Optional Future)

Potential webhook events for third-party integrations:
- `book.borrowed`
- `book.returned`
- `book.overdue`
- `reservation.ready`
- `fine.generated`
- `member.registered`

---

**Last Updated:** 2026-04-09
