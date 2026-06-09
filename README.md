# 🏫 School Management System

[![Electron](https://img.shields.io/badge/Electron-42.2.0-47848F?style=flat&logo=electron)](https://www.electronjs.org/)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=flat&logo=react)](https://react.dev/)
[![Django](https://img.shields.io/badge/Django-REST-092E20?style=flat&logo=django)](https://www.django-rest-framework.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13+-336791?style=flat&logo=postgresql)](https://www.postgresql.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-339933?style=flat&logo=nodedotjs)](https://nodejs.org/)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat)]()

---

> A full-stack school management system delivered as a Windows desktop application via Electron. Handles student enrollment, academic tracking, financial transactions, and administrative reporting.

**Electron Compatibility:** v42.2.0 (compatible with Windows 7+)

---

## Project Overview

| Aspect | Value |
|--------|-------|
| **App Name** | School Management System |
| **App ID** | `com.sms.schoolmanagementsystem` |
| **Default School Name** | My School |
| **Default Short Name** | SMS |
| **Default Tagline** | School Management System |
| **Electron Version** | 42.2.0 |
| **Theme** | Light only (no dark mode) |

---

## Technology Stack

| Layer | Technology |
|-------|------------|
| **Backend** | Django REST Framework |
| **Database** | PostgreSQL |
| | User: `postgres` / Password: `darkzhen` / DB: `SMSS` |
| **Frontend** | React 18 + Vite + Tailwind CSS |
| **Desktop Shell** | Electron v42.2.0 |
| **Icons** | Lucide React |
| **Authentication** | Token-based (DRF `authtoken`) |
| **Admin Theme** | Django Jazzmin |

---

## Repository Structure

```
System Project/
├── README.md                  <- this file
├── frontend/                  <- React + Vite + Electron app
│   ├── electron/
│   ├── src/
│   │   ├── components/
│   │   ├── contexts/         <- AuthContext, SchoolConfigContext
│   │   ├── layouts/
│   │   ├── pages/            <- 18 page components
│   │   ├── services/         <- api.js (Django REST client)
│   │   ├── App.jsx
│   │   └── index.jsx
│   ├── dist/                 <- Built output (gitignored)
│   ├── package.json
│   └── README.md
└── school_pos/               <- Django backend
    ├── manage.py
    ├── school_pos/           <- Project settings (incl. JAZZMIN_SETTINGS)
    ├── school_config/        <- Singleton school configuration app
    │   ├── models.py         <- SchoolConfiguration
    │   ├── views.py          <- SchoolConfigView (GET/PUT/PATCH)
    │   ├── serializers.py
    │   ├── urls.py           <- /api/school-config/current/
    │   ├── admin.py          <- SchoolConfigurationAdmin
    │   ├── context_processors.py  <- exposes school_name to templates
    │   ├── templates/admin/  <- Custom admin branding
    │   └── management/commands/reset_school_config.py
    ├── students/             <- Student management
    ├── finance/              <- Payments, tuition, revenue
    ├── academic/             <- Sections, tracks, courses
    ├── facilities/           <- Dormitory
    ├── staff/                <- Staff portal
    └── .venv/                <- Python virtual environment
```

---

## Backend Setup

### Prerequisites
- Python 3.10+
- PostgreSQL 13+
- Node.js 18+ (for frontend)

### Database
Create the PostgreSQL database:
```sql
CREATE DATABASE "SMSS" WITH OWNER postgres ENCODING 'UTF8';
```

### Install & Run
```bash
cd "C:\Users\DarkZhen\Documents\System Project\school_pos"
.venv\Scripts\activate

pip install -r requirements.txt

# Apply migrations
python manage.py migrate

# Create a superuser
python manage.py createsuperuser

# Run the dev server
python manage.py runserver 0.0.0.0:8000
```

### School Configuration Commands
```bash
# Reset school config to generic defaults (My School / SMS)
python manage.py reset_school_config

# Create a new migration after model changes
python manage.py makemigrations school_config
python manage.py migrate school_config
```

---

## Frontend Setup

### Install
```bash
cd "C:\Users\DarkZhen\Documents\System Project\frontend"
npm install
```

### Development
```bash
npm run dev
# Opens at http://localhost:5173
```

### Production Build (Web only)
```bash
npm run build
# Output: frontend/dist/
```

### Electron Build (Windows)
```bash
npm run build:win
# Output: frontend/dist/
#   - USAT School Management System Setup 1.0.0.exe  (NSIS installer)
#   - win-unpacked/                                  (x64)
#   - win-ia32-unpacked/                             (ia32)
```

> Note: Despite the executable name, the project is now generic. The `name` field in `package.json` is still labeled "usat-school-management-system" for compatibility; update it if you want a different executable name.

---

## Backend API

- **Base URL:** `http://localhost:8000/api/`
- **Auth:** Token-based (`Authorization: Token <token>`)
- **Token storage:** `localStorage.authToken` (axios interceptor)

### Key Endpoints

| Endpoint | Description |
|----------|-------------|
| `/api/auth/login/` | Login, returns token |
| `/api/students/` | Student CRUD |
| `/api/sections/`, `/api/tracks/`, `/api/courses/` | Academic structure |
| `/api/tuition-payments/` | Tuition records |
| `/api/payments/` | Walk-in & general payments |
| `/api/revenue/` | Revenue reports |
| `/api/school-config/current/` | School configuration (singleton) |

---

## Core Features

### 1. Student Management
- Student enrollment with LRN tracking (12-digit validation)
- Student profiles with personal & guardian information
- Sections, Tracks (strands), and Courses management
- Student assessments and grading
- Dropout tracking with reinstatement flow
- Graduation event management
- Dormitory room assignments
- School tour scheduling and enrollments

### 2. Finance & Cashier
The Cashier is the main financial transaction hub:

- **Walk-in Customer Support** - Parents, guardians, or authorized payers can make payments without being a student
- **Multiple Payment Types:** Tuition, Miscellaneous, Laboratory, Assessment, Other
- **Payment Methods:** Cash, GCash, Bank Transfer, Card, Check
- **Receipt Generation** showing school name/address, payer, itemized description, payment method, reference number, timestamp
- **Searchable Transaction History** - Search by payer name, reference number
- **Pending Payments Tracking** - Monitor outstanding balances
- **Quick Payment Form** with Student/Walk-in toggle
- **Cash change calculation** for cash payments

### 3. Revenue Reports
- Financial summary: Total Income, General Payments, Total Expenses, Collection Rate
- Revenue breakdown by category (Tuition, Miscellaneous, Laboratory, Assessment, Dorm, Tour, Graduation, POS, Other)
- Top paying students listing with medal indicators
- Print-friendly A4 landscape reports
- Date-range, type, and method filters
- CSV export functionality

### 4. Sales
- Product sales tracking
- Revenue from non-tuition sources

---

## School Configuration (Singleton)

The system uses a **singleton** configuration model (`SchoolConfiguration`, `pk=1`) to manage school-wide branding exposed globally to both Django templates and the React frontend.

### Django Templates
A `school_config` context processor exposes `school_name`, `school_logo_url`, and `school_short_name` to all templates. Custom `templates/admin/base_site.html` and `templates/admin/login.html` override the Jazzmin default branding.

### React Frontend
A `SchoolConfigContext` (`src/contexts/SchoolConfigContext.jsx`) provides:
- `schoolName` (default: "My School")
- `schoolShortName` (default: "SMS")
- `schoolLogo`
- `schoolTagline`

These are used in the sidebar, page titles, receipts, and the `TitleUpdater` component that sets `document.title` per route.

### Update via API
```
PATCH /api/school-config/current/
Content-Type: application/json  (or multipart/form-data for logo upload)
```

To change the school name, logo, contact info, etc. at runtime, use the **School Config** page in the application or call the API directly.

---

## Navigation Structure

| Section | Pages |
|---------|-------|
| **STUDENT** | Students, Sections, Tracks, Courses, Enroll, Dropouts |
| **FINANCE** | Cashier, Tuition, Revenue, Pending Payments, Sales |
| **ACADEMICS** | Assessments, Graduation, Tours |
| **FACILITIES** | Dorm |
| **SYSTEM** | Settings, School Config, Profile |

---

## UI/UX Decisions

### Light-Only Theme
- All `dark:` Tailwind variants removed
- `darkMode` removed from `tailwind.config.js`
- `ThemeContext` returns a stub `{ isDark: false, toggleTheme: () => {} }`
- `<html>` is force-stripped of the `dark` class on load
- No theme toggle in the Settings page
- Light palette: `bg-white` (cards), `bg-gray-50` (page), `border-gray-200`, `text-gray-900` primary, `text-gray-500` muted

### Layout
- Stat cards: horizontal flex row, icon top-left, bold value, muted label below
- Tables: rounded `rounded-2xl` containers, compact padding, hover states, empty states with icons
- Modals: backdrop blur, sticky headers, X close buttons
- Toast notifications: slide-up animation, success (gray-800) / error (red-600), 3.5s auto-dismiss

### Print Formats
- **80mm thermal receipt** for cashier transactions
- **A4 official receipt** for accounting
- **A4 enrollment form** (long bond paper) for student registration

---

## Pages (18 total)

| Route | Page |
|-------|------|
| `/login` | LoginPage |
| `/` | DashboardPage |
| `/students` | StudentsPage |
| `/sections` | SectionsPage |
| `/tracks` | TracksPage |
| `/courses` | CoursesPage |
| `/enroll` | EnrollPage |
| `/dropouts` | DropoutsPage |
| `/cashier` | CashierPage |
| `/tuition` | TuitionPage |
| `/revenue` | RevenuePage |
| `/pending-payments` | PendingPaymentsPage |
| `/sales` | SalesPage |
| `/assessments` | AssessmentsPage |
| `/graduation` | GraduationPage |
| `/tours` | ToursPage |
| `/dorm` | DormPage |
| `/settings` | SettingsPage |
| `/school-config` | SchoolConfigPage |
| `/profile` | ProfilePage |

---

## Known Notes

> [!NOTE]
> **ThemeContext** is a stub kept for API compatibility — all pages render in light mode.

> [!NOTE]
> **SchoolConfiguration** is a singleton (`pk=1`). Use `python manage.py reset_school_config` to revert to defaults.

> [!NOTE]
> **NSIS installer** requires admin rights to install (Windows per-machine symlinks).

> [!IMPORTANT]
> **Payment method values** are case-sensitive: `Cash`, `Card`, `GCash`, `Bank Transfer`, `Check`.

> [!NOTE]
> The product executable filename in `package.json` is still labeled "usat-school-management-system" for backward compatibility. Update the `name`, `productName`, and `appId` fields in `package.json` to rebrand.

---

## Development Status

| Feature | Status |
|---------|--------|
| Light-only theme across all 18 pages | ✅ Done |
| School configuration singleton (frontend + backend) | ✅ Done |
| School branding in admin (Django + Jazzmin) and cashier receipts | ✅ Done |
| Walk-in customer support in Cashier | ✅ Done |
| Receipt shows school name and item correctly | ✅ Done |
| Revenue reports with horizontal stat layout | ✅ Done |
| Electron production build (x64 + ia32) with `app.getAppPath()` fix | ✅ Done |
| Migrations applied and school config reset to generic defaults | ✅ Done |
| NSIS installer built successfully | ✅ Done |

---

## License

This project is licensed under the **MIT License**.

---

*Made with ❤️ for schools everywhere*
