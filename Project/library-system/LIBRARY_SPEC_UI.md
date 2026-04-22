# UI/UX Guidelines

## Design Principles

### 1. Simplicity First
- Minimize clicks to accomplish tasks
- Avoid technical jargon
- Clear, descriptive labels
- One action per screen/section

### 2. Accessibility
- WCAG 2.1 AA compliance
- High contrast ratios
- Font size minimum 14px (body text)
- Touch-friendly (mobile: 44px+ tap targets)
- Keyboard navigation support
- Screen reader compatible

### 3. Bilingual Design
- Language toggle (top right)
- All text in English AND Bahasa Malaysia
- Consistent terminology in both languages
- Right-to-left layout support (if needed for Malay)

### 4. Non-Tech-Savvy Users
- Icons with text labels
- Confirmation dialogs for important actions
- Clear error messages (not technical codes)
- Help tooltips on complex features
- Undo options where possible

### 5. Mobile Responsiveness
- Mobile-first design approach
- Responsive breakpoints: 320px, 768px, 1024px, 1440px
- Touch-optimized interface
- Readable on all devices

---

## Layout Specifications

### Desktop Layout (1024px+)

```
┌─────────────────────────────────────────────┐
│  Logo | Search Bar | User Menu | Language  │
├──────────────────────────────────────────────┤
│       │                                      │
│ Nav   │           Main Content               │
│       │                                      │
│       │                                      │
├──────────────────────────────────────────────┤
│              Footer                          │
└──────────────────────────────────────────────┘
```

### Mobile Layout (320px-767px)

```
┌──────────────────────┐
│ ☰ | Search | User    │
├──────────────────────┤
│                      │
│   Main Content       │
│   (Full Width)       │
│                      │
├──────────────────────┤
│      Footer          │
└──────────────────────┘
```

---

## Navigation

### Top Navigation Bar
- **Left:** Library logo/home link
- **Center:** Search bar (visible on desktop, hamburger menu on mobile)
- **Right:** 
  - Language toggle (EN | MS)
  - Notifications bell (if logged in)
  - User menu (profile, logout)

### Left Sidebar (Desktop Only)
**For Members:**
- 📚 My Books (Active Loans)
- 🔖 My Reservations
- 💰 My Fines
- 👤 My Profile
- ⚙️ Settings

**For Staff:**
- 🏠 Dashboard
- 📚 Books Management
- 👥 Member Search
- 💳 Check-in/Check-out
- 💰 Fine Management
- 📊 Reports
- ⚙️ Settings (Admin only)

### Mobile Navigation
- Hamburger menu (3 lines icon)
- Slide-out sidebar
- Auto-collapse after selection

---

## Color Scheme

### Primary Colors
```
Primary Blue:     #1E40AF (Main actions, links)
Secondary Teal:   #0D9488 (Accents, success)
Dark Gray:        #1F2937 (Text, headers)
Light Gray:       #F3F4F6 (Backgrounds, borders)
```

### Semantic Colors
```
Success:   #10B981 (Green - confirmation, available)
Warning:   #F59E0B (Amber - due soon, caution)
Error:     #EF4444 (Red - overdue, unavailable)
Info:      #3B82F6 (Blue - information, help)
```

### Accessibility
- Minimum contrast ratio: 4.5:1 for normal text
- Don't rely on color alone to convey meaning
- Use icons + color for status indicators

---

## Typography

### Font Stack
```css
Body:    Segoe UI, Tahoma, sans-serif
Headers: Segoe UI, Tahoma, sans-serif (bold)
Mono:    Courier New, monospace (for barcodes, IDs)
```

### Font Sizes
```
H1 (Page Title):    28px, Bold
H2 (Section):       20px, Bold
H3 (Subsection):    16px, Bold
Body Text:          14px, Regular
Small Text/Help:    12px, Regular
Labels:             13px, Medium
```

### Line Height
```
Body text:  1.6
Headers:    1.2
Lists:      1.8
```

---

## Button Design

### Button States
```
Primary Button (Main Actions):
  Normal:    Blue background, white text, rounded corners
  Hover:     Darker blue, slight shadow
  Active:    Darker blue, pressed effect
  Disabled:  Gray background, lighter text, no cursor

Secondary Button (Alternative Actions):
  Normal:    White/transparent, blue border, blue text
  Hover:     Light blue background
  Active:    Blue background, white text

Danger Button (Delete, Suspend):
  Normal:    Red background, white text
  Hover:     Darker red, shadow
  Active:    Pressed effect

Size:       44px height minimum (touch-friendly)
Spacing:    16px padding (left/right)
Border:     2px rounded corners
```

### Button Labels
```
Primary Actions:   Save, Borrow, Reserve, Renew, Check Out
Secondary:        Cancel, Back, Close
Destructive:      Delete, Remove, Suspend, Waive (for fines)
States:           Loading (spinner), Disabled (grayed out)
```

---

## Form Design

### Input Fields
```
Label:          Above field, bold, required indicator (*)
Field:          32px height, 8px padding, gray border
Placeholder:    Light gray, helpful hint text
Focus:          Blue border (2px), subtle shadow
Error:          Red border, error message below
Success:        Green checkmark (optional)
```

### Validation Rules
```
Real-time validation:    Email, username (no waiting)
On-blur validation:      After user leaves field
On-submit validation:    Final check before sending
Error messages:          Clear, actionable, not technical
```

### Example Form
```
┌─────────────────────────────────┐
│ Register New Member             │
├─────────────────────────────────┤
│ Full Name *                     │
│ [________________]              │
│                                 │
│ Email Address *                 │
│ [________________]              │
│ 📧 must be valid email         │
│                                 │
│ Phone Number *                  │
│ [________________]              │
│                                 │
│ Member Type *                   │
│ [Registered        ▼]           │
│                                 │
│ Address                         │
│ [________________]              │
│                                 │
│ Preferred Language              │
│ ○ English  ○ Bahasa Malaysia    │
│                                 │
│ [  Register  ]  [  Cancel  ]    │
└─────────────────────────────────┘
```

---

## Cards & Content Blocks

### Book Card (Search Results)
```
┌────────────────────────┐
│ [Book Cover Image]     │
├────────────────────────┤
│ Learning PHP           │
│ David Sklar            │
│ ★★★★★ (5 reviews)     │
│                        │
│ Available: 2 of 3      │
│                        │
│ [View Details] [Borrow]│
└────────────────────────┘
```

### Loan Card (My Books)
```
┌────────────────────────────┐
│ Learning PHP               │
│ Due: April 30, 2026       │
│ ████████░░ 18 days left   │
│                           │
│ ⚠️ 3 days until due       │
│                           │
│ [Renew]  [View Details]   │
└────────────────────────────┘
```

### Fine Card
```
┌────────────────────────────┐
│ Overdue Fine               │
│ Book: Laravel Guide        │
│ Amount: RM 10.00          │
│ Status: UNPAID            │
│                           │
│ [Pay Now]  [View Details] │
└────────────────────────────┘
```

---

## Pages & Flows

### 1. Home Page (Unauthenticated)
**Hero Section:**
- Library name/logo
- Search bar (prominent)
- Quick links: Browse Catalog, Member Login, Register

**Featured Section:**
- New arrivals (6 books)
- Trending books
- Library hours & contact
- Call-to-action: "Register for Free"

### 2. Catalog/Search Page
**Left Sidebar (Desktop):**
- Search filter by category
- Language filter
- Availability filter
- Sort options

**Main Content:**
- Search results grid (responsive)
- Book cards with cover images
- Pagination controls
- Results count

### 3. Book Details Page
**Content:**
- Book cover image (large)
- Title, Author, Publisher
- Description (bilingual tabs)
- Ratings & Reviews section
- Availability status
- Borrowing rules (period, renewal limit)

**Actions (if logged in):**
- [Borrow] button (if available)
- [Reserve] button (if unavailable)
- [Add to Wishlist] (optional)

### 4. Member Dashboard
**For Members:**
- Quick stats: Books borrowed, Fines owed, Reservations
- Active loans card (due dates highlighted)
- Active reservations
- Fine balance
- Recent activity

**For Staff (Extra Sections):**
- Quick checkout/return panel
- Member search
- Daily circulation
- System alerts

### 5. Checkout Process (Staff)
**Step 1: Find Member**
- Barcode scanner input
- Search by ID/name
- Display member details
- Confirm member

**Step 2: Add Books**
- Scan book barcode
- Display book details
- Click "Add to Cart"
- Repeat for multiple books

**Step 3: Confirm & Print**
- Show all items being borrowed
- Due date display
- Print receipt
- Email receipt
- Confirmation message

---

## Responsive Design Breakpoints

```
Mobile:         320px - 767px
Tablet:         768px - 1023px
Desktop:        1024px - 1439px
Large Desktop:  1440px+
```

### Adjustments by Breakpoint
```
Mobile (≤767px):
- Single column layout
- Full-width buttons
- Hamburger navigation
- Larger touch targets (44px+)
- Stacked cards

Tablet (768px-1023px):
- 2-column content where appropriate
- Visible sidebar (collapsed initially on small tablets)
- Grid books: 2-3 per row
- Horizontal scrolling for tables (with wrapper)

Desktop (1024px+):
- 3+ column layouts
- Visible sidebar with navigation
- Grid books: 4-6 per row
- Full tables without scrolling
```

---

## Dialogs & Modals

### Confirmation Dialog
```
┌──────────────────────────┐
│ Confirm Action           │
├──────────────────────────┤
│ Are you sure you want    │
│ to return this book?     │
│                          │
│ [Cancel]  [Confirm]      │
└──────────────────────────┘
```

### Error Message
```
┌──────────────────────────┐
│ ⚠️ Error                 │
├──────────────────────────┤
│ Unable to borrow book.   │
│ You have exceeded your   │
│ item limit (5 items).    │
│                          │
│ Please return a book     │
│ before borrowing more.   │
│                          │
│ [Close]                  │
└──────────────────────────┘
```

### Success Notification
```
┌──────────────────────────┐
│ ✓ Success                │
├──────────────────────────┤
│ Book borrowed            │
│ successfully!            │
│ Due: April 30, 2026     │
└──────────────────────────┘
(Auto-closes after 3 seconds)
```

---

## Loading States

### Skeleton Loaders
```
Book Card Loading:
┌────────────────────┐
│ [████████████]     │  <- Gray placeholder
├────────────────────┤
│ [██████████]       │  <- Title placeholder
│ [██████]           │  <- Author placeholder
│ [████████]         │  <- Rating placeholder
└────────────────────┘
```

### Spinner
- Centered spinner during long operations
- With loading message (e.g., "Loading books...")

---

## Barcode Scanner UX

### Scanner Input Field
```
┌─────────────────────────────────┐
│ Scan barcode or enter ID:       │
│ [_______________________] 🎯     │
│ (Auto-focus, accepts pasted     │
│  barcodes from scanner)         │
├─────────────────────────────────┤
│ Scanned: 9781491927282         │
│ Processing...                   │
└─────────────────────────────────┘
```

### Feedback
- Visual & audio confirmation on successful scan
- Error message if barcode not found
- Auto-advance to next field
- Clear previous scan option

---

## Accessibility Features

### Keyboard Navigation
- Tab through all interactive elements
- Enter/Space to activate buttons
- Arrow keys for dropdowns/menus
- Escape to close modals

### Screen Reader Support
- Semantic HTML (buttons, links, labels)
- ARIA labels for icons
- Form labels associated with inputs
- Alt text for images
- Skip navigation link

### Color Blindness
- Don't use color alone (add icons/patterns)
- Test with color blindness simulator
- Use patterns for status (✓, ✗, ⚠️)

---

## Localization (Bilingual)

### Content
- All text strings externalized
- Translation management system
- Date format: DD/MM/YYYY (Malaysian standard)
- Currency: RM (Malaysian Ringgit)
- Phone format: +6012-3456789

### Styling
- Support for both LTR (English) and potential RTL
- Sufficient padding for longer translations
- Font support for Malaysian characters (already in Unicode)

---

**Last Updated:** 2026-04-09
