# Hospital Management System (HMS)

> **Enterprise Healthcare Administration Platform**  
> Developed for **Parul University — Faculty of IT & Computer Science**  
> **Degree Program:** Master of Computer Applications (MCA) — Semester III, A.Y. 2026–2027  
> **Student Author:** Mayankraj Kumawat (Enrollment No.: 2505112120078)  
> **Project Guide:** Prof. Priyanka Mod  

[![Django 5.2](https://img.shields.io/badge/Django-5.2-092E20?logo=django&logoColor=white)](https://www.djangoproject.com/)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Bootstrap 5.3](https://img.shields.io/badge/Bootstrap-5.3-7952B3?logo=bootstrap&logoColor=white)](https://getbootstrap.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Build Status](https://img.shields.io/badge/Tests-54%2F54%20Passing-success)](#testing)

---

## 🏥 Project Overview

The **Hospital Management System (HMS)** is a full-featured, secure, and production-ready web application engineered to centralize hospital operations. It addresses the inefficiencies of paper-based hospital registers and fragmented spreadsheets by providing an integrated solution for:

- **Patient Demographics & Electronic Records:** Automatic sequential ID generation (`PAT-YYYY-NNNN`), soft-delete with active appointment guards, search, and emergency contact tracking.
- **Doctor Profiles & Availability Scheduling:** Department taxonomies, consultation fee configuration, and weekly recurring OPD slot grids.
- **Double-Booking Protected Appointments:** Database-level unique constraints preventing scheduling collisions on `(doctor, date, time_slot)` with automated status workflows (`SCHEDULED`, `CONFIRMED`, `COMPLETED`, `CANCELLED`).
- **Electronic Medical Records & Prescriptions:** Consultation diagnoses, symptoms, treatments, inline prescription item management, and append-only audit revision logs.
- **Dynamic Billing & ReportLab PDF Invoices:** Live line-item calculation (`subtotal + tax - discount`), overpayment prevention, and auto-generated downloadable PDF invoices.
- **Role-Scoped Dashboards with Chart.js:** Real-time KPIs, financial trends, and appointment status distributions tailored for each role.

---

## 👥 Role-Based Access Control (RBAC)

HMS strictly enforces permissions **server-side** using custom Django mixins (`RoleRequiredMixin`) and decorators (`@role_required`).

| Role | Access Scope |
| :--- | :--- |
| **Administrator (`ADMIN`)** | Full system oversight: user accounts management, doctor onboarding, financial revenue analytics, audit trails, and configuration. |
| **Doctor (`DOCTOR`)** | Assigned OPD schedules, consultation queue, electronic medical record and prescription entry, and patient clinical histories. |
| **Receptionist (`RECEPTIONIST`)** | Patient registration and profile management, appointment booking and rescheduling, itemized billing, and payment processing. |
| **Patient (`PATIENT`)** | Self-service portal: book consultations, view upcoming and past appointments, view medical diagnoses/prescriptions, and review invoice payment statuses. |

---

## 🛠️ Technology Stack

- **Backend:** Python 3.11+, Django 5.2 (Model-View-Template architecture)
- **Frontend:** HTML5, CSS3, JavaScript (Vanilla ES6+), Bootstrap 5.3, Bootstrap Icons, Chart.js
- **Form Handling:** `django-crispy-forms` + `crispy-bootstrap5`
- **PDF Generation:** ReportLab 3.x
- **Database:** SQLite 3 (Development & Automated Testing) / PostgreSQL-ready (Production)
- **Static Assets:** WhiteNoise 6.x (Compressed Manifest Storage)
- **Environment Management:** `python-dotenv`

---

## 📂 Project Structure

```
Hospital Management/
├── apps/
│   ├── accounts/          # Custom User model, auth views, RBAC mixins, ActivityLog, rate limiter
│   ├── patients/          # Patient demographic CRUD, PAT-YYYY-NNNN auto-IDs, soft-delete guard
│   ├── doctors/           # Doctor profiles, department taxonomy, weekly availability slots
│   ├── appointments/      # Consultation booking, status lifecycle, double-booking guard constraint
│   ├── medical_records/   # Clinical diagnoses, prescriptions, append-only RecordAudit trail
│   ├── billing/           # Invoice creation, dynamic line items, payments, ReportLab PDF engine
│   └── dashboard/         # Role-specific KPI metrics, Chart.js trends, query count optimization
├── config/
│   ├── settings.py        # Django configuration with environment-driven security headers
│   ├── urls.py            # Global routing, custom 404/500 handlers, media/static configs
│   ├── views.py           # Global error handlers (404 and 500)
│   └── wsgi.py / asgi.py
├── docs/
│   ├── ERD.md             # Mermaid Entity Relationship Diagram
│   ├── ROUTES.md          # Comprehensive endpoint route and permission matrix
│   └── hospital_management_master_prd.md # Full academic PRD specification
├── static/
│   ├── css/               # Modern design system (styles.css, print.css)
│   └── js/                # Client-side dynamic behaviours (main.js, Chart.js init)
├── templates/
│   ├── base.html          # Responsive base layout with role-aware sidebar and top navigation
│   ├── partials/          # Reusable components (form_field, pagination, confirm_modal, toast)
│   ├── 404.html / 500.html# Custom styled error pages
│   └── [app_templates]/   # Specialized templates for each app
├── .env.example           # Template for environment variables
├── CONTRIBUTING.md        # Commit conventions and developer guide
├── manage.py              # Django management script
└── requirements.txt       # Pinned project dependencies
```

---

## ⚡ Quickstart Guide

### 1. Clone & Set Up Virtual Environment

```bash
# Clone the repository
git clone https://github.com/your-username/hospital-management.git
cd "hospital-management"

# Create and activate Python virtual environment
python -m venv venv

# Windows (PowerShell):
.\venv\Scripts\Activate.ps1
# Linux / macOS:
source venv/bin/activate
```

### 2. Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 3. Configure Environment Variables

Copy the sample environment file:
```bash
cp .env.example .env
```

Contents of `.env`:
```ini
DEBUG=True
SECRET_KEY=django-insecure-parul-hms-secret-key-change-in-production-2026
ALLOWED_HOSTS=localhost,127.0.0.1
```

### 4. Run Migrations

```bash
python manage.py makemigrations
python manage.py migrate
```

### 5. Seed Realistic Demo Data

Populate the database with complete sample records (1 Admin, 2 Doctors, 2 Receptionists, 5 Patients, 10 Appointments, 5 Medical Records, 5 Bills):

```bash
python manage.py seed_demo
```

### 6. Launch the Development Server

```bash
python manage.py runserver
```

Open your browser and navigate to: **[http://127.0.0.1:8000/](http://127.0.0.1:8000/)**

---

## 🔑 Demo Credentials

All test accounts seeded by `python manage.py seed_demo`:

| Role | Username | Password | Dashboard Link |
| :--- | :--- | :--- | :--- |
| **Administrator** | `admin` | `admin123` | `/dashboard/admin/` |
| **Doctor (Cardiology)** | `doctor1` | `doctor123` | `/dashboard/doctor/` |
| **Doctor (Pediatrics)** | `doctor2` | `doctor123` | `/dashboard/doctor/` |
| **Receptionist** | `reception1` | `recep123` | `/dashboard/receptionist/` |
| **Receptionist** | `reception2` | `recep123` | `/dashboard/receptionist/` |
| **Patient** | `patient1` | `patient123` | `/dashboard/patient/` |
| **Patient** | `patient2` | `patient123` | `/dashboard/patient/` |

---

## 🧪 Testing

The test suite covers model validation, form submissions, RBAC security matrix, double-booking prevention, invoice recalculation, and cross-cutting security features.

### Run All Unit & Integration Tests:
```bash
python manage.py test --verbosity=2
```

### Run Module-Specific Test Suites:
```bash
python manage.py test apps.accounts -v 2
python manage.py test apps.patients -v 2
python manage.py test apps.doctors -v 2
python manage.py test apps.appointments -v 2
python manage.py test apps.medical_records -v 2
python manage.py test apps.billing -v 2
python manage.py test apps.dashboard -v 2
```

### Security & Deployment Readiness Check:
```bash
python manage.py check --deploy
```

---

## 🔒 Security Hardening

- **Brute-Force Rate Limiting:** Built-in middleware throttles repeat login requests to a maximum of 5 attempts per 10-minute window per IP (HTTP 429).
- **PBKDF2 Password Hashing:** Industry-standard password encryption with high iteration count.
- **Audit Trails:** Sensitive changes (clinical record amendments, soft deletions, payment receipts) are recorded with timestamps, user IDs, and client IP addresses.
- **Production SSL & Cookie Isolation:** `SECURE_SSL_REDIRECT`, `SESSION_COOKIE_SECURE`, `CSRF_COOKIE_SECURE`, `SECURE_HSTS_SECONDS`, and `X-Frame-Options` automatically activate when `DEBUG=False`.

---

## 🎓 Academic Acknowledgements

Special gratitude to **Prof. Priyanka Mod** for project supervision and architectural guidance throughout the development lifecycle of this Hospital Management System at **Parul University**.
