# 🎓 Student Information Portal (Fadderiet)

[![CI / CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue?logo=githubactions&logoColor=white)](#ci--devops-workflows)
[![Backend](https://img.shields.io/badge/Backend-Django%20%7C%20Django%20REST%20Framework-green?logo=django&logoColor=white)](#backend-architecture)
[![Frontend](https://img.shields.io/badge/Frontend-React%20%7C%20TypeScript%20%7C%20Vite-61DAFB?logo=react&logoColor=black)](#frontend-architecture)
[![Containerization](https://img.shields.io/badge/Orchestration-Docker%20%7C%20Docker%20Compose-2496ED?logo=docker&logoColor=white)](#containerization--local-setup)
[![Security](https://img.shields.io/badge/Security-Passwordless%20OTP-red)](#security--architecture-highlights)
[![Code Quality](https://img.shields.io/badge/Linting-Flake8%20%7C%20ESLint-yellow)](#testing--quality-assurance)

> A modular, containerized full-stack web platform built for university student orientation and mentorship coordination. Designed with role-based access control, passwordless email authentication (OTP), dynamic content publishing, and batch user onboarding.

---

## 📌 Project Overview

The **Student Information Portal** is a distributed web application developed as a Bachelor’s Capstone Project by a 9-engineer agile scrum team at **Linköping University**. The platform streamlines communication, event coordination, document distribution, and group management between university orientation coordinators (*Faddrar*), program administrators, and new students.

### Key Capabilities
* **Role-Based Access Control (RBAC):** Distinct permission tiers for students, coordinators (*faddrar*), and portal administrators.
* **Passwordless Authentication:** Frictionless and secure login flow using Email-based One-Time Passwords (OTP) with short-lived token validation, eliminating password fatigue and credential stuffing risks.
* **Batch Ingestion Pipeline:** Automated user and student group bulk onboarding from structured datasets.
* **Dynamic Event & Post Distribution:** Real-time information feeds, activity schedules, and tagging architecture.
* **Secure Document Delivery:** In-portal viewing and access management for orientation guides and PDF assets.

---

## 🛠️ System Architecture & Tech Stack

~~~text
                     ┌────────────────────────────────────────┐
                     │          Client Browser Layer          │
                     │   React (TSX) + Tailwind + Vite SPA    │
                     └───────────────────┬────────────────────┘
                                         │  HTTPS / REST APIs
                                         ▼
                     ┌────────────────────────────────────────┐
                     │           Django REST Backend          │
                     │  ┌──────────────────┬────────────────┐ │
                     │  │ Security & Auth  │  Portal Logic  │ │
                     │  │ (OTP/Token/RBAC) │  (CRUD/Feeds)  │ │
                     │  └──────────────────┴────────────────┘ │
                     └───────────────────┬────────────────────┘
                                         │  ORM Persistence
                                         ▼
                     ┌────────────────────────────────────────┐
                     │           Relational Database          │
                     │           PostgreSQL / SQLite          │
                     └────────────────────────────────────────┘
~~~

### Core Technologies

| Domain | Technology Stack |
| :--- | :--- |
| **Frontend** | React 18, TypeScript, Vite, Tailwind CSS, PostCSS, Lucide/Icons |
| **Backend** | Python 3.x, Django 5.x, Django REST Framework (DRF), ASGI/WSGI |
| **Security & Auth** | Passwordless Email OTP, Token Authentication, RBAC Permissions |
| **Testing** | Vitest, React Testing Library, Django Test Suite, Coverage.py |
| **DevOps & CI/CD** | Docker, Docker Compose, GitHub Actions, Flake8, ESLint |

---

## 🔐 Security & Architecture Highlights

### 1. Passwordless Authentication Flow (`fad_backend/security`)
* User login triggers a time-limited One-Time Password (OTP) sent via a custom SMTP email utility (`EmailSender.py`).
* OTP codes expire automatically to mitigate replay attacks, managed natively via the `two_factor_code.py` model with `expires_at` validation.
* Successful code submission returns authenticated session credentials (`login_view.py`, `generate_token_view.py`), completely bypassing traditional password management.

### 2. Modular Backend Design (`fad_backend/portal`)
* **Data Models:** Granular entities for `user.py`, `program.py`, `group.py`, `post.py`, `post_pdf.py`, `post_link.py`, and `calender.py`.
* **Dynamic Tagging Engine:** Multi-tag support for filtering posts, schedules, and materials by program or cohort.
* **Batch Import Utility:** Custom views for batch-registering student users (`import_users_view.py`) and assigning initial access groups seamlessly.

### 3. Reactive, Type-Safe Frontend (`fad_frontend/src`)
* Custom data-fetching hooks (`useApi.ts`, `useSmartState.ts`) for uniform error handling, cache state, and loading spinners.
* Componentized design separating UI elements (`ActivityCard.tsx`, `FolderContentCard.tsx`, `ProfileModals.tsx`) from administrative views (`ConfigurePage.tsx`, `OverviewPage.tsx`, `ShareInfoPage.tsx`).

---

## 🚀 Getting Started (Docker Quickstart)

The entire environment (backend, frontend, and dependencies) is containerized for deterministic local deployment.

### Prerequisites
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) (v20.10+)
* [Docker Compose](https://docs.docker.com/compose/) (v2.0+)
* [Git](https://git-scm.com/)

### Running with Docker Compose

1. **Clone the repository:**
   ~~~bash
   git clone https://github.com/Puffen8/student-information-portal-bachelors-project.git
   cd student-information-portal-bachelors-project
   ~~~

2. **Start the application services:**
   ~~~bash
   docker compose up --build
   ~~~

3. **Access the portal:**
   * Frontend: `http://localhost:5173`
   * Backend API: `http://localhost:8000/api/`
   * Django Admin: `http://localhost:8000/admin/`

4. **Initialize seed data (Optional):**
   ~~~bash
   docker compose exec backend python manage.py initportal
   ~~~

---

## 🧪 Testing & Code Quality

### Backend Quality Suite
~~~bash
# Run Django Unit Tests & Coverage
docker compose exec backend coverage run manage.py test portal security
docker compose exec backend coverage report

# Run PEP8 Linting
docker compose exec backend flake8 .
~~~

### Frontend Quality Suite
~~~bash
# Navigate to frontend directory
cd fad_frontend

# Run Vitest Suite
npm run test

# Run ESLint validation
npm run lint
~~~

---

## ⚙️ CI / DevOps Workflows

Continuous Integration is enforced via **GitHub Actions** (`.github/workflows/backend.yml`):
* **Automated Linting:** Flake8 and ESLint checks run on push/pull request.
* **Test Isolation:** Spin-up of dedicated Docker Compose testing containers (`docker-compose.django-test.yml`, `docker-compose.django-lint.yml`).
* **Test Coverage Enforcement:** Prevents regressions in serializers, views, and authentication models.

---

## 👥 Engineering & Collaboration Context

This project was built collaboratively by a **team of 9 software engineering students** at **Linköping University (LiU)** as a Bachelor's Degree Capstone Project, utilizing Agile/Scrum methodology, code reviews, and structured CI pipelines.
