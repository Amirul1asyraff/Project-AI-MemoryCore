# Technology Stack

## Backend Stack

### Framework & Core
| Component | Technology | Version | Why |
|-----------|-----------|---------|-----|
| **Framework** | Laravel | 11.x | Best-in-class Laravel ecosystem, excellent documentation, rapid development |
| **PHP** | PHP | 8.2+ | Latest stable version, strong type hints, performance |
| **Database** | MySQL | 8.0+ | Reliable, proven, excellent for library systems, easy to backup |
| **ORM** | Eloquent | (Laravel built-in) | Intuitive, powerful relationships, migrations built-in |

### Authentication & Security
| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Auth Framework** | Laravel Breeze (Vue) | Simple, secure, pre-built authentication with Vue SPA |
| **API Auth** | Laravel Sanctum | SPA authentication + API token management |
| **Roles & Permissions** | Spatie Laravel Permission | RBAC (Admin, Librarian, Front Desk, Member) |
| **Password Hashing** | Bcrypt | Laravel default, secure password storage |
| **CORS** | Laravel Sanctum middleware | Stateful SPA authentication |
| **Rate Limiting** | Laravel middleware | Prevent abuse and DDoS attacks |

### APIs & Services
| Component | Package | Purpose |
|-----------|---------|---------|
| **Email** | Laravel Mail + Mailable | Send notifications and confirmations |
| **SMTP** | Gmail/SendGrid/Local | Email delivery |
| **File Storage** | Laravel Storage | Manage book cover images, documents |
| **Queues** | Laravel Queue (Redis/Database) | Async tasks (email, reports) |
| **Caching** | Redis/File Cache | Cache catalog searches, reduce DB load |

### Utilities & Packages
| Component | Package | Purpose |
|-----------|---------|---------|
| **Validation** | Laravel Validator | Server-side input validation, form rules |
| **Localization** | Laravel i18n + Vue i18n | Bilingual support (English & Malay) |
| **Carbon** | Carbon (Laravel built-in) | Date/time manipulation |
| **UUID** | `ramsey/uuid` | Member ID generation |
| **HTTP Client** | Guzzle (Laravel built-in) | Make HTTP requests |
| **Media Library** | Spatie Laravel MediaLibrary | Book cover image uploads & management |
| **Activity Log** | Spatie Laravel Activitylog | Audit trail for all operations |

### Development Tools
| Tool | Purpose |
|------|---------|
| **Laravel Tinker** | REPL for testing code |
| **Laravel Telescope** | Debug requests, queries, performance |
| **Laravel Debugbar** | Development debugging (dev only) |
| **PHPStan** | Static code analysis |
| **PHP-CS-Fixer** | Code formatting |
| **Vue DevTools** | Vue component debugging |
| **ESLint** | JavaScript/Vue linting |
| **Prettier** | Code formatting for JS/Vue |

---

## Frontend Stack

### Core Technologies
| Component | Technology | Purpose |
|-----------|-----------|---------|
| **SPA Framework** | Vue.js 3 | Modern reactive UI, component-based architecture |
| **State Management** | Pinia | Centralized state management for Vue 3 |
| **Routing** | Vue Router | Client-side routing for SPA |
| **Styling** | Tailwind CSS | Utility-first, rapid development |
| **Icons** | Heroicons | Consistent, beautiful icons |
| **Build Tool** | Vite | Fast build tool, HMR for development |
| **Package Manager** | npm | Frontend dependency management |

### Vue.js Integration
| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Laravel Integration** | Laravel Breeze (Vue) | Pre-built auth scaffolding with Vue |
| **API Communication** | Axios | HTTP client for API calls |
| **Form Handling** | VeeValidate | Client-side form validation |
| **UI Components** | Headless UI | Accessible, unstyled components |

### Browser Compatibility
```
Minimum Support:
- Chrome/Edge: Latest 2 versions
- Firefox: Latest 2 versions
- Safari: Latest 2 versions
- Mobile browsers: iOS Safari 12+, Chrome Android 80+

Progressive Enhancement:
- Basic functionality works without JavaScript
- Enhanced with JavaScript/CSS3 features
```

---

## Database Schema & Migrations

### Migration Strategy
```bash
Database Version Control:
- Laravel Migrations (located in database/migrations/)
- Auto-increment for data consistency
- Named migrations: {timestamp}_{action_description}.php
- Always include reverse operations (down() method)

Backup Strategy:
- Daily automated backups (AWS RDS or manual mysqldump)
- Weekly full database dumps
- Retention: 30 days
- Off-site backup storage
```

### Performance Optimization
| Optimization | Implementation |
|--------------|-----------------|
| **Indexes** | Added to: isbn, barcode, due_date, user_id, status |
| **Caching** | Redis for catalog searches, member lookups |
| **Pagination** | 20 items per page (configurable) |
| **Query Optimization** | Eager loading (with) to prevent N+1 queries |
| **Full-text Search** | MySQL FULLTEXT index on books (title, author) |

---

## Deployment Architecture

### Server Requirements

**Minimum Specs:**
```
CPU:     2 cores
RAM:     2GB
Storage: 50GB (for ~20,000 books)
Bandwidth: 5 Mbps
```

**Recommended Specs:**
```
CPU:     4 cores
RAM:     4-8GB
Storage: 100GB
Bandwidth: 10 Mbps
Load Balancer: Optional (future)
```

### Server Stack
| Component | Technology | Version |
|-----------|-----------|---------|
| **OS** | Ubuntu/Linux | 20.04 LTS or newer |
| **Web Server** | Nginx | 1.18+ |
| **PHP** | PHP-FPM | 8.2+ |
| **Database** | MySQL | 8.0+ or PostgreSQL 13+ |
| **Cache** | Redis | 6.0+ (optional, improves performance) |
| **Process Manager** | Supervisor | Manage Laravel queue workers |

### Deployment Options

#### Option 1: Shared Hosting (Smallest Scale)
```
Provider: Laravel-friendly hosting
(Bluehost, SiteGround, DigitalOcean App Platform)
Cost: $15-50/month
Best for: Initial launch, testing
```

#### Option 2: VPS (Recommended)
```
Provider: DigitalOcean, Linode, AWS Lightsail, Vultr
Cost: $6-20/month
OS: Ubuntu 20.04 LTS
Setup: Manual or via script (Laravel Forge)
Advantages: Full control, scalable
```

#### Option 3: Managed Services (Most Reliable)
```
Database: AWS RDS (managed MySQL)
App: Laravel Forge + DigitalOcean
Storage: AWS S3 (book cover images)
CDN: CloudFlare
Cost: $30-100/month
Advantages: Automatic backups, scaling, monitoring
```

---

## Development Workflow

### Local Development Environment
```bash
Requirements:
- PHP 8.2+
- Composer
- MySQL 8.0 or SQLite (local)
- Node.js 18+ (for npm/Vite)

Setup (Laravel + Vue SPA):
1. git clone <repo>
2. composer install
3. npm install
4. cp .env.example .env
5. php artisan key:generate
6. php artisan migrate --seed
7. npm run dev (Vite dev server with HMR)
8. php artisan serve (API backend)

Access:
- Frontend: http://localhost:5173 (Vite)
- Backend API: http://localhost:8000 (Laravel)
```

### Testing Tools
```bash
Backend Testing:
- PHPUnit (unit & feature tests)
- Laravel Dusk (browser testing)
- Pest (modern testing alternative)

Frontend Testing:
- Vitest (Vue unit tests)
- Vue Test Utils (component testing)
- Cypress/Playwright (E2E testing)

Code Quality:
- PHPStan (static analysis)
- PHP-CS-Fixer (code formatting)
- Laravel Pint (opinionated formatter)
- ESLint + Prettier (JS/Vue)
```

---

## Third-Party Integrations

### Email Service
```
Primary: Gmail SMTP or SendGrid
Backup: Local mail server
Configuration: .env file
Rate: Unlimited (configurable queue)

Notification Types:
- Transactional (must deliver)
- Bulk (non-critical, can queue)
```

### Barcode Scanner Integration
```
Hardware: Standard USB HID barcode scanner
Protocol: Keyboard input emulation
Format: EAN-13 (ISBN)
No special driver needed (keyboard simulation)

Implementation:
- HTML <input> field with auto-focus
- JavaScript event listener for barcode input
- Real-time validation on scan
```

### File Storage
```
Local Storage:
- Server's /storage/app/uploads/ directory
- Book cover images
- Generate thumbnails on upload

Alternative (Future):
- AWS S3 for scalability
- CloudFlare CDN for faster delivery
```

---

## Monitoring & Performance

### Application Monitoring
| Tool | Purpose |
|------|---------|
| **Laravel Telescope** (dev) | Debug requests, queries, logs |
| **Sentry** (optional) | Error tracking in production |
| **New Relic/DataDog** (optional) | Performance monitoring |

### Database Monitoring
```
- Slow query log (MySQL)
- Query execution times
- Index usage statistics
- Connection pool monitoring
```

### Server Monitoring
```
- CPU & Memory usage
- Disk space
- Network bandwidth
- Uptime/downtime tracking
```

### Performance Targets
```
Page Load Time: < 2 seconds
API Response Time: < 500ms
Database Query Time: < 100ms
Search Results: < 1 second
```

---

## Security Technologies

### Data Protection
| Layer | Technology |
|-------|-----------|
| **Transport** | HTTPS/TLS 1.2+ |
| **Database** | Encrypted connections (SSL) |
| **At Rest** | Database encrypted backups |
| **Sessions** | HTTP-only, Secure cookies |

### Authentication & Authorization
```
- JWT tokens for API
- Session cookies for web
- RBAC (Role-Based Access Control)
- Password reset via email
```

### Input Validation & Sanitization
```
- Server-side validation on all inputs
- Parameterized queries (prevent SQL injection)
- XSS protection (auto-escape in Blade)
- CSRF protection (token on forms)
```

---

## Backup & Disaster Recovery

### Backup Strategy
```
Database:
- Daily automated backups
- Retention: 30 days
- Off-site storage (AWS S3, Google Drive)
- Test restore weekly

Application Files:
- Git version control
- GitHub/GitLab (private repo)
- Auto-backup on each commit

Media (Book Covers):
- Included in file backups
- Regenerate thumbnails from originals
```

### Recovery Plan
```
RPO (Recovery Point Objective): 1 day
RTO (Recovery Time Objective): 4 hours

Steps:
1. Identify issue
2. Restore from latest backup
3. Verify data integrity
4. Resume operations
5. Document incident
```

---

## Scaling Considerations (Future)

### Phase 1 (Current - 2,000 members)
- Single server
- MySQL on same server or managed RDS
- Local file storage
- No caching layer needed

### Phase 2 (5,000+ members)
- Load balancer
- Multiple app servers
- Separate database server
- Redis caching layer
- CDN for static files

### Phase 3 (10,000+ members)
- Database read replicas
- Session clustering
- Message queue system (RabbitMQ)
- Microservices consideration
- Database sharding (future)

---

## Cost Estimation (Annual)

### Minimal Setup
```
Web Hosting:        RM 600/year
Database (RDS):     RM 500/year
Email Service:      Free (Gmail SMTP)
Storage/Backup:     RM 200/year
Domain:             RM 50/year
─────────────────────────
Total:              ~RM 1,350/year
```

### Recommended Setup
```
VPS (DigitalOcean): RM 2,400/year
Database (Managed): RM 1,200/year
Email (SendGrid):   RM 500/year
CDN (CloudFlare):   RM 600/year
Monitoring:         RM 400/year
Domain/SSL:         RM 150/year
─────────────────────────
Total:              ~RM 5,250/year
```

### Premium Setup (High Availability)
```
Load Balancer:      RM 2,000/year
Multiple Servers:   RM 7,200/year
Database (Master):  RM 2,400/year
Database (Replica): RM 2,400/year
Email Service:      RM 1,000/year
CDN Premium:        RM 1,200/year
Monitoring/Alerts:  RM 1,500/year
Backup Service:     RM 800/year
─────────────────────────
Total:              ~RM 18,500/year
```

---

**Last Updated:** 2026-04-09
