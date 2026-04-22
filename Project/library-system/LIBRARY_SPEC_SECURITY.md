# Security Considerations

## Security Overview

This document outlines all security measures, best practices, and compliance requirements for the Library Management System.

---

## 1. Authentication & Authorization

### 1.1 User Authentication

#### Password Security
```
Requirements:
- Minimum 8 characters
- Must contain: uppercase, lowercase, number, special character
- Bcrypt hashing (Laravel default)
- Salt automatically generated per password
- Never store plaintext passwords
- No password expiration (industry best practice, focus on breach response)

Password Reset:
- Email-based token (valid for 60 minutes only)
- One-time use token
- Invalidate old tokens on new request
- Require new password entry
```

#### Session Management
```
Session Configuration:
- Session timeout: 8 hours (configurable)
- Idle timeout: 1 hour
- HTTP-only cookies (prevent XSS access)
- Secure flag (HTTPS only)
- SameSite: Strict (prevent CSRF)

Token Management (JWT):
- Issued on login
- Expiration: 1 hour
- Refresh token: 7 days
- Revoked on logout
- Renewed automatically before expiration
```

### 1.2 Role-Based Access Control (RBAC)

#### Permission Model
```
Roles:
- Admin (Full system access)
- Librarian (Catalog & reports)
- Front Desk (Checkout/return operations)
- Member (Self-service access)
- Guest (Read-only catalog)

Permissions:
- Fine-grained permission checks
- Route-level protection (middleware)
- Model-level authorization (policies)
- Action-level validation

Implementation:
- Laravel Authorization gates & policies
- Middleware protection on all routes
- Verify permissions on every action
```

#### Data Access Control
```
Members Can Access:
- Own profile only
- Own borrowing history
- Own fines
- Own reservations
- Public catalog

Members CANNOT Access:
- Other members' information
- Staff reports
- System settings
- Financial data
- Admin functions

Staff Access:
- Varies by role (see ROLES.md)
- Logged and audited
- Cannot access personal data unnecessarily
```

---

## 2. Data Protection

### 2.1 Encryption

#### In Transit (Network)
```
HTTPS/TLS Configuration:
- TLS 1.2 minimum (preferably 1.3)
- Strong cipher suites only
- Certificate: Let's Encrypt (auto-renewal)
- HSTS header enabled
- No mixed content (all HTTPS)

Implementation:
- Force redirect HTTP → HTTPS
- HSTS: max-age=31536000; includeSubDomains
- Test with SSL Labs (A+ grade minimum)
```

#### At Rest (Database)
```
Database Connections:
- SSL connection required
- Encrypted credentials in .env
- Never hardcode sensitive data

Sensitive Data Fields:
- Passwords: Bcrypt hashed (not encrypted)
- Email addresses: Plain (necessary for functionality)
- Phone numbers: Plain (necessary for functionality)
- Addresses: Plain (necessary for functionality)
- Fine amounts: Plain (no encryption needed)

Optional Encryption:
- Member ID numbers (encrypted with key)
- Barcode data (encrypted if needed)
```

#### Backup Encryption
```
Backup Requirements:
- All backups encrypted
- Encryption key separate from backups
- Key stored securely (not on server)
- Test decrypt regularly

Backup Storage:
- Off-site location (AWS S3 with encryption)
- Access restricted (minimal staff)
- Versioning enabled
- Retention: 30 days
```

### 2.2 Data Retention & Deletion

#### Retention Policy
```
User Data:
- Active members: Keep indefinitely
- Inactive members (5+ years): Soft delete (archive)
- Suspended members: 1 year after resolution
- Staff accounts: 3 years after deletion

Borrowing Records:
- Active: Keep indefinitely
- Historical (returned books): 7 years (legal requirement)
- Preserve for legal/audit purposes

Fine Records:
- All records: 7 years (legal requirement)
- Paid fines: Archive after 1 year
- Waived fines: 7 years (audit trail)

Audit Logs:
- Critical actions: 3 years
- General logs: 90 days
- Error logs: 30 days
```

#### Data Deletion (GDPR/Privacy)
```
Member Right to Deletion:
- Request deletion of personal data
- System should soft-delete (retain for legal)
- Anonymize personally identifiable info
- Exception: Legal/financial records (7 years)

Deletion Process:
1. Manager reviews request
2. Legal check (any pending issues)
3. Data anonymization
4. Archive legal records
5. Confirmation to member

Audit:
- Log all deletions
- Reason documented
- Approved by manager
```

---

## 3. Input Validation & Sanitization

### 3.1 Input Validation

#### Server-Side Validation (REQUIRED)
```
CRITICAL: Never trust client-side validation alone

Validation Rules:
- Email: RFC 5322 format
- Phone: Malaysian format (01x-xxxx xxxx)
- Names: Max 255 chars, no special characters
- Descriptions: Max 5000 chars
- ISBN: 13-digit, numeric only
- Barcode: 13-digit, numeric only

Implementation:
- Laravel Validator
- Custom validation rules
- Real-time feedback
- Error messages in user language
```

#### Data Type Enforcement
```
Type Checking:
- ID fields: Integer, > 0
- Dates: Valid date format
- Currency: Decimal (2 places)
- Enums: Specific values only
- Booleans: True/False only

Example:
$validated = $request->validate([
    'email' => 'required|email:rfc',
    'phone' => 'required|regex:/^01[0-9]-[0-9]{4} [0-9]{4}$/',
    'fine_amount' => 'required|decimal:2',
    'member_type' => 'required|in:registered,senior,student',
]);
```

### 3.2 Output Sanitization

#### HTML Escaping
```
Blade Template Output:
- {{ $variable }} → Auto-escaped (safe)
- {!! $variable !!} → Unescaped (use carefully)

Rules:
- Always escape user input in templates
- Only use {!! !!} for trusted content
- Use e() helper if needed in PHP

Example:
<!-- SAFE -->
{{ $user->name }}  <!-- Auto-escaped -->

<!-- UNSAFE - Don't do this -->
{!! $user->name !!}  <!-- Only if trusted source -->
```

#### JSON Output
```
API Response Sanitization:
- All user input escaped in JSON
- No XSS payloads in responses
- Validate before outputting

Example:
return response()->json([
    'success' => true,
    'message' => htmlspecialchars($input),
]);
```

---

## 4. SQL Injection Prevention

### 4.1 Parameterized Queries

#### Eloquent ORM (SAFE)
```
GOOD - Use Eloquent:
$user = User::where('email', $request->email)->first();
$books = Book::whereIn('category', $categories)->get();

SAFE - Use bindings:
$users = DB::select('SELECT * FROM users WHERE email = ?', [$email]);

DANGEROUS - String concatenation:
$users = User::where('email', $email . "'")  // NO!
$query = "SELECT * FROM users WHERE email = '$email'";  // NO!
```

#### Query Logging & Auditing
```
Track all queries:
- Log parameterized queries
- Monitor for suspicious patterns
- Alert on unusual activity
- Regular review of logs
```

---

## 5. Cross-Site Scripting (XSS) Prevention

### 5.1 Output Encoding

#### Template Escaping
```
Blade Directives:
{{ $var }}           → HTML escaped (safe)
{!! $var !!}         → Raw output (use carefully)
@js($var)            → JavaScript escaped
@json($data)         → JSON encoded

Example:
<!-- Safe -->
<p>{{ $book->title }}</p>

<!-- Unsafe - Don't do -->
<p>{!! $book->title !!}</p>

<!-- If rendering user HTML -->
{!! $htmlContent !!}  <!-- Must be sanitized first -->
```

#### HTML Purification (Optional)
```
For User-Generated Content:
- Use HTMLPurifier library
- Strip all dangerous HTML tags
- Allow only safe tags (p, b, i, a, etc.)

Example:
$purifier = new HTMLPurifier();
$cleanHTML = $purifier->purify($userInput);
```

### 5.2 Content Security Policy (CSP)
```
HTTP Headers:
Content-Security-Policy: default-src 'self'; 
                        script-src 'self' 'unsafe-inline';
                        style-src 'self' 'unsafe-inline';
                        img-src 'self' data:;

Implementation:
- Set in Nginx/Apache headers
- Or use Laravel middleware
- Regularly audit CSP violations
```

---

## 6. Cross-Site Request Forgery (CSRF) Prevention

### 6.1 CSRF Token Implementation

#### Token Generation & Validation
```
Laravel CSRF Protection (Built-in):
- Automatic CSRF token generation
- Token included in session
- Validated on state-changing requests (POST, PUT, DELETE)
- Token expires on logout

Implementation:
<!-- In forms -->
@csrf

<!-- In AJAX requests -->
headers: {
  'X-CSRF-TOKEN': document.querySelector('meta[name="csrf-token"]').content
}

<!-- In API calls -->
Accept: application/json
Content-Type: application/json
X-CSRF-TOKEN: {{ csrf_token() }}
```

#### SameSite Cookie Attribute
```
Configuration:
session.http_only = true
session.secure = true
session.same_site = 'strict'

Effect:
- Prevents cross-site cookie sending
- Strong CSRF protection
- Modern browser support
```

---

## 7. Security Headers

### 7.1 HTTP Security Headers

```
Headers to Implement:
┌────────────────────────────────────────────┐
│ X-Content-Type-Options: nosniff            │ Prevent MIME sniffing
│ X-Frame-Options: SAMEORIGIN                │ Prevent clickjacking
│ X-XSS-Protection: 1; mode=block            │ Legacy XSS protection
│ Strict-Transport-Security: max-age=...     │ Force HTTPS
│ Referrer-Policy: strict-origin-when-...    │ Control referrer
│ Permissions-Policy: geolocation=()         │ Restrict APIs
└────────────────────────────────────────────┘

Implementation (Nginx):
add_header X-Content-Type-Options "nosniff" always;
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```

---

## 8. Authentication Attacks Prevention

### 8.1 Brute Force Protection

#### Rate Limiting
```
Login Attempts:
- Max 5 attempts per 15 minutes (per IP)
- Max 10 attempts per 24 hours (per username)
- Progressive delays (exponential backoff)
- IP-based blocking after threshold

Implementation:
Route::post('/login', 'AuthController@login')
    ->middleware('throttle:5,15|throttle:10,1440');

Redis Configuration:
- Track attempts in Redis
- Fast lookups, automatic expiration
- Alert on suspicious patterns
```

#### Account Lockout
```
Temporary Lockout:
- After 5 failed attempts: 15-minute lockout
- After 10 failed attempts: 1-hour lockout
- Notify user via email
- Allow reset via email token

Configuration:
LOGIN_ATTEMPTS_LIMIT = 5
LOGIN_ATTEMPTS_TIMEOUT = 15 (minutes)
LOCKOUT_DURATION = 3600 (seconds)
```

### 8.2 Session Hijacking Prevention

```
Session Security:
- Regenerate session ID on login
- Bind session to user agent
- Bind session to IP address (optional)
- Detect unusual activity

Implementation:
session_regenerate_id(true);

Detection:
- Monitor for session changes
- Alert on different IP/browser
- Force re-authentication if suspicious
- Log all session changes
```

---

## 9. API Security

### 9.1 API Authentication

#### JWT Token Security
```
Token Configuration:
- Algorithm: HS256 or RS256
- Secret key: 256+ bits (generate with openssl)
- Expiration: 1 hour (short-lived)
- Refresh token: 7 days (long-lived)

Implementation:
HEADER: Authorization: Bearer {token}

Token Structure:
{
  "iss": "library-api",
  "sub": 1,  // user_id
  "iat": 1704067200,
  "exp": 1704070800,
  "role": "member"
}

Refresh Flow:
1. Access token expires
2. Send refresh token
3. Validate refresh token
4. Issue new access token
5. Optional: Issue new refresh token
```

### 9.2 API Rate Limiting

```
Rate Limits by Role:
- Unauthenticated: 60 requests/hour/IP
- Member: 1000 requests/hour
- Staff: 5000 requests/hour
- Admin: 10000 requests/hour

Implementation:
Route::middleware('auth:api', 'throttle:1000,60')
  ->group(function () {
    // Member routes
  });

Response Headers:
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1704154000
```

### 9.3 API Input Validation

```
All API endpoints validate:
- Required fields present
- Data types correct
- Field lengths reasonable
- Special characters escaped
- No SQL injection attempts
- No file upload attacks

Example:
POST /api/books
{
  "title": "Learning PHP",          // String, 1-500 chars
  "author": "David Sklar",           // String, 1-255 chars
  "isbn": "9781491927282",           // 13-digit ISBN
  "category": "Technology"           // Enum value
}
```

---

## 10. File Upload Security

### 10.1 Book Cover Image Upload

#### File Validation
```
Allowed File Types:
- Image only: .jpg, .jpeg, .png, .webp
- MIME type validation
- File extension validation
- Magic number validation (file signature)

Size Limits:
- Maximum file size: 5MB
- Recommended: 2MB
- Resize on upload (optimize)

Implementation:
$request->validate([
    'cover_image' => 'image|mimes:jpeg,png,webp|max:5120',
]);

Additional Checks:
- Check MIME type (not just extension)
- Verify image dimensions
- Re-encode image (removes malicious code)
- Store outside web root (if possible)
```

#### File Storage
```
Storage Location:
- /storage/app/uploads/book-covers/
- Outside public web directory
- Served through controller (validate access)
- Backup included in database backups

Filename Security:
- Never use user-provided filename
- Generate random filename: {uuid}.jpg
- Store original filename in database
- Protect directory: no .htaccess execution
```

---

## 11. Logging & Monitoring

### 11.1 Security Logging

#### Events to Log
```
Authentication:
✓ Successful login
✓ Failed login attempts
✓ Password reset
✓ Session timeout
✓ Logout

Authorization:
✓ Permission denied attempts
✓ Role changes
✓ Account suspension
✓ Data access by staff

Data Changes:
✓ Book additions/deletions
✓ Member modifications
✓ Fine waivers
✓ Bulk operations

System:
✓ Configuration changes
✓ Error/exceptions
✓ Database backups
✓ Security policy updates
```

#### Log Content
```
Each Log Entry:
- Timestamp (precise)
- User ID & username
- Action/Event type
- Resource affected (book_id, member_id)
- Old value → New value (if change)
- IP address & User agent
- Status (success/failure)
- Error message (if failed)

Example:
[2026-04-09 14:35:22] user_id=5 action=borrow_book 
book_id=123 status=success ip=192.168.1.100
```

### 11.2 Monitoring & Alerts

#### Real-Time Alerts
```
Trigger alerts for:
- 10+ failed login attempts (from one IP)
- Member suspension attempts
- Fine waiver requests (review required)
- Bulk data deletions
- Database errors
- Backup failures
- SSL certificate expiration (30 days)

Alert Recipients:
- Admin email
- System administrator
- Operations team
```

#### Regular Reviews
```
Daily:
- Failed login attempts
- System errors
- API rate limit violations

Weekly:
- User access patterns
- Permission changes
- New member registrations

Monthly:
- Security audit log
- Backup verification
- SSL certificate status
- Dependency updates
```

---

## 12. Vulnerability Management

### 12.1 Dependency Updates

```
Automated Scanning:
- Dependabot (GitHub)
- Laravel security advisories
- CVE database monitoring
- PHP security updates

Update Policy:
- Critical: Within 24 hours
- High: Within 1 week
- Medium: Within 2 weeks
- Low: Within 1 month

Testing After Update:
- Run full test suite
- Regression testing
- Manual testing
- Staging deployment first
```

### 12.2 Security Scanning

#### Static Code Analysis
```
Tools:
- PHPStan (strict mode)
- PHAN (static analyzer)
- SonarQube (optional)
- OWASP Dependency Check

Process:
- Run on every commit (CI/CD)
- Fail build on high-severity issues
- Review medium/low issues
- Document any false positives
```

#### Dynamic Testing
```
Tools:
- OWASP ZAP (penetration testing)
- Burp Suite (optional)
- Manual security testing
- Barcode scanner testing

Frequency:
- Before production launch
- Quarterly security audits
- After major changes
```

---

## 13. Compliance & Standards

### 13.1 Data Protection Compliance

#### Malaysian Privacy Requirements
```
Personal Data Protection Act (PDPA):
- Obtain consent before collecting data
- Use data only for stated purposes
- Protect data from unauthorized access
- Allow member requests for access/deletion
- Notify of any data breaches
- Privacy policy clearly stated

Implementation:
- Privacy policy on website
- Consent checkbox on registration
- Data retention policy documented
- Breach notification procedure
```

### 13.2 Security Standards

#### OWASP Top 10 (2021)
```
Protection against:
1. Broken Access Control → RBAC implementation
2. Cryptographic Failures → HTTPS, encryption
3. Injection → Parameterized queries, validation
4. Insecure Design → Security by design
5. Security Misconfiguration → Hardened defaults
6. Vulnerable Components → Dependency updates
7. Authentication Failures → Strong auth system
8. Data Integrity Failures → Input validation
9. Logging Failures → Comprehensive logging
10. SSRF → Input validation, URL whitelist
```

---

## 14. Incident Response

### 14.1 Security Incident Procedure

```
1. Detection & Containment (First Hour)
   - Identify the issue
   - Isolate affected systems
   - Stop active attacks
   - Preserve evidence

2. Investigation (Next 24 Hours)
   - Determine scope
   - Affected data/users
   - Root cause analysis
   - Timeline reconstruction

3. Notification (Per PDPA)
   - Notify affected members
   - Explain what happened
   - Provide remediation steps
   - Offer credit monitoring (if needed)

4. Remediation (Next 48-72 Hours)
   - Fix vulnerability
   - Apply patches
   - Strengthen defenses
   - Conduct security audit

5. Recovery & Documentation
   - Monitor for re-attack
   - Update security policies
   - Training for staff
   - Public communication
   - Post-incident review
```

### 14.2 Breach Notification

```
Notification Timeline:
- Immediately: Internal notification
- 24-48 hours: Affected member notification
- 7 days: Regulatory notification (if required)

Notification Content:
- What data was affected
- When the breach occurred
- What we're doing about it
- What members should do
- Contact for questions
- Available support

Contact Information:
- Security email: security@library.example.com
- Hotline: +60-X-XXXX-XXXX
- Website: library.example.com/security
```

---

## 15. Security Checklist (Pre-Launch)

- [ ] HTTPS configured (TLS 1.2+)
- [ ] Security headers implemented
- [ ] CSRF protection enabled
- [ ] Input validation on all fields
- [ ] Output escaping in templates
- [ ] SQL injection prevention verified
- [ ] XSS prevention tested
- [ ] Authentication system working
- [ ] Authorization checks in place
- [ ] Password hashing (Bcrypt)
- [ ] Session security configured
- [ ] Rate limiting enabled
- [ ] Logging system functional
- [ ] Backups encrypted & tested
- [ ] Sensitive data not in logs
- [ ] API authentication secure
- [ ] Error messages don't leak info
- [ ] File uploads validated
- [ ] Dependencies updated
- [ ] Security scan passed
- [ ] Penetration testing completed
- [ ] Privacy policy published
- [ ] PDPA compliance verified
- [ ] Incident response plan ready
- [ ] Staff security training complete
- [ ] Disaster recovery tested

---

## 16. Security Contact

```
Report Security Issues:
Email: security@library.example.com
Responsible Disclosure:
- Do not publicly disclose
- Allow 90 days to fix
- We will acknowledge within 24 hours
- We will provide updates every 2 weeks
```

---

**Last Updated:** 2026-04-09  
**Security Level:** HIGH  
**Classification:** Internal Use
