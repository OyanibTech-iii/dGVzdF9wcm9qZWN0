# TRUNO's Beauty Salon (S Beauty Salon) - Symfony Web Application

## Overview

TRUNO's Beauty Salon (also branded as S Beauty Salon) is a web application for beauty salon management and online appointment booking, built with Symfony 8.1 and PHP 8.4+. It represents the modern Symfony migration of the legacy PHP salon management system, featuring a customer-facing storefront for exploring treatments and booking appointments, alongside a full administrative dashboard for managing bookings, services, customers, users, and system activity logs.

By default, the application can run immediately without a database connection using built-in in-memory service providers (`SalonService` and `AdminService`) and session-based slot reservation tracking, while also supporting Doctrine ORM and PostgreSQL/MySQL for persistent data storage.

---

## Table of Contents

1. [Key Features](#key-features)
2. [Technology Stack](#technology-stack)
3. [System Requirements](#system-requirements)
4. [Project Directory Structure](#project-directory-structure)
5. [Full Setup and Installation Guide](#full-setup-and-installation-guide)
   - [Step 1: Clone or Navigate to the Project](#step-1-clone-or-navigate-to-the-project)
   - [Step 2: Install PHP Dependencies](#step-2-install-php-dependencies)
   - [Step 3: Configure Environment Variables](#step-3-configure-environment-variables)
   - [Step 4: Install Frontend Assets](#step-4-install-frontend-assets)
   - [Step 5: Database Setup (Optional for Demo Mode)](#step-5-database-setup-optional-for-demo-mode)
   - [Step 6: Start the Local Development Server](#step-6-start-the-local-development-server)
6. [Application Routes and Endpoints](#application-routes-and-endpoints)
   - [Public Storefront Routes](#public-storefront-routes)
   - [Administrative Dashboard Routes](#administrative-dashboard-routes)
7. [Running Automated Tests](#running-automated-tests)
8. [Troubleshooting](#troubleshooting)
9. [License](#license)

---

## Key Features

### Public Storefront
- **Home Landing Page**: Displays featured treatment starting prices, salon highlights, and verified client testimonials.
- **Service Catalog**: Categorized breakdown of treatments (Make up, Hair styling, Nail care, Cosmetology, and SPA procedures) with detailed descriptions, durations, and pricing.
- **Interactive Appointment Booking**: Enables customers to select a service, specialist, preferred date, and time slot. Includes CSRF protection and session-based slot collision detection.
- **Live Slot Availability API**: JSON endpoint (`/api/slots`) that dynamically checks and returns available time slots per staff member, date, and service.
- **About Us and Team Roster**: Showcases the salon's history, philosophy, and specialist profiles.
- **Contact Form**: Allows prospective and existing clients to submit inquiries with flash notification feedback.

### Administrative Dashboard
- **Executive Overview**: Displays real-time Key Performance Indicators (KPIs) including total bookings, active services, registered clients, user accounts, and activity log counts.
- **Appointment Management**: View, filter, update details, change statuses (Pending, Confirmed, Completed, Cancelled), or delete appointments.
- **Service Catalog Management**: Create, update, and remove salon treatments, prices, durations, and category assignments.
- **Customer Directory**: Searchable table of customer profiles and contact information.
- **User Account Administration**: Manage system users, assign roles (`ROLE_ADMIN`, `ROLE_STAFF`, `ROLE_CUSTOMER`), and toggle account active status.
- **Activity Logs**: Audit trail tracking booking events, status changes, catalog modifications, and administrator sign-ins.
- **Interactive DataTables**: All administrative listings integrate DataTables 3.1.5 for client-side searching, sorting, pagination, and category/status filtering.

---

## Technology Stack

- **Framework**: Symfony 8.1
- **Language**: PHP 8.4 or higher
- **Templating Engine**: Twig 3.x
- **Frontend Asset Management**: Symfony AssetMapper, Stimulus, and Symfony UX Turbo
- **ORM and Database**: Doctrine ORM 3.7, Doctrine Migrations 4.0 (PostgreSQL 16 via Docker Compose, or MySQL/MariaDB)
- **Data Grid UI**: DataTables 3.1.5
- **Testing Framework**: PHPUnit 13.4 with Symfony BrowserKit and CSS Selector

---

## System Requirements

Ensure the following tools are installed on your machine before setting up the project:

| Tool / Component | Minimum Version | Recommended Version | Notes |
| :--- | :--- | :--- | :--- |
| **PHP** | `8.4.0` | `8.4.x` | Required extensions: `ctype`, `iconv`, `session`, `pdo` |
| **Composer** | `2.5+` | Latest `2.x` | PHP dependency manager |
| **Symfony CLI** | `5.x` | Latest `5.x` | Optional, recommended for local web server |
| **Docker & Docker Compose** | `24.0+` | Latest | Optional, for running the PostgreSQL container |

---

## Project Directory Structure

```text
symfony/
├── assets/                     # Frontend assets managed by AssetMapper
│   ├── controllers/            # Stimulus JavaScript controllers
│   ├── images/                 # Static WebP and image assets
│   ├── styles/                 # Application stylesheets
│   ├── app.js                  # Main JavaScript entrypoint
│   └── stimulus_bootstrap.js   # Stimulus initialization
├── bin/
│   ├── console                 # Symfony CLI console executable
│   └── phpunit                 # PHPUnit test runner wrapper
├── config/                     # Framework, bundle, route, and service configuration
│   ├── packages/               # Package-specific YAML configs
│   ├── bundles.php             # Registered Symfony bundles
│   ├── routes.yaml             # Route configuration
│   └── services.yaml           # Dependency injection service definitions
├── migrations/                 # Doctrine database migration classes
├── public/
│   └── index.php               # Web server front controller
├── src/
│   ├── Controller/
│   │   ├── DashboardController.php  # Admin dashboard routes and actions
│   │   └── HomeController.php       # Public pages, booking handler, and slot API
│   ├── Entity/                 # Doctrine ORM entities
│   ├── Repository/             # Doctrine repositories
│   ├── Service/
│   │   ├── AdminService.php    # Administrative data and management service
│   │   └── SalonService.php    # Public catalog, specialists, and slot service
│   └── Kernel.php              # Symfony application kernel
├── templates/                  # Twig view templates
│   ├── dashboard/              # Admin dashboard views (overview, services, users, etc.)
│   ├── home/                   # Public storefront views (index, about, booking, etc.)
│   ├── partials/               # Reusable header, footer, and UI partials
│   └── base.html.twig          # Master HTML layout template
├── tests/
│   ├── Controller/
│   │   └── DashboardControllerTest.php  # Functional HTTP tests for dashboard pages
│   └── bootstrap.php           # Test environment bootstrap
├── var/                        # Generated cache, logs, and shared runtime files
├── compose.yaml                # Docker Compose configuration for PostgreSQL
├── composer.json               # PHP package dependencies and scripts
├── importmap.php               # Frontend JavaScript importmap definitions
└── phpunit.dist.xml            # PHPUnit configuration
```

---

## Full Setup and Installation Guide

Follow these steps to configure and run the Symfony application locally.

### Step 1: Clone or Navigate to the Project

Open a terminal (PowerShell, Command Prompt, or Bash) and navigate to the `symfony` directory inside the workspace:

```bash
cd c:\Users\pacif\shaina\symfony
```

### Step 2: Install PHP Dependencies

Install all required vendor packages and trigger Symfony Flex auto-scripts using Composer:

```bash
composer install
```

### Step 3: Configure Environment Variables

The repository includes a default `.env` file. To customize settings for your local machine without modifying committed files, create a `.env.local` file:

```bash
cp .env .env.local
```

Open `.env.local` and ensure `APP_SECRET` is set to a secure random string:

```dotenv
APP_ENV=dev
APP_SECRET=your_custom_32_character_secret_key_here
DEFAULT_URI=http://localhost:8000
```

If you plan to connect a database (optional for initial UI and functional testing, as `SalonService` and `AdminService` provide built-in sample data out of the box), configure `DATABASE_URL` in `.env.local`:

- **For PostgreSQL (default in `.env`)**:
  ```dotenv
  DATABASE_URL="postgresql://app:!ChangeMe!@127.0.0.1:5432/app?serverVersion=16&charset=utf8"
  ```
- **For MySQL / MariaDB (e.g., XAMPP)**:
  ```dotenv
  DATABASE_URL="mysql://root:@127.0.0.1:3306/truno_beauty_salon?serverVersion=8.0.32&charset=utf8mb4"
  ```

### Step 4: Install Frontend Assets

Install the JavaScript importmap packages and publish public bundle assets:

```bash
php bin/console importmap:install
php bin/console assets:install public
```

### Step 5: Database Setup (Optional for Demo Mode)

The application is pre-configured with in-memory service providers (`App\Service\SalonService` and `App\Service\AdminService`) so that all public pages, booking flows, and dashboard views run immediately without requiring an active database server.

If you are developing persistent Doctrine entities and want to start the database:

1. **Using Docker Compose (PostgreSQL 16)**:
   ```bash
   docker compose up -d
   ```
2. **Create the database and run migrations**:
   ```bash
   php bin/console doctrine:database:create --if-not-exists
   php bin/console doctrine:migrations:migrate --no-interaction
   ```

*(Note: If you are also running the legacy PHP application located in `../legacy`, its MySQL schema and seed data are located in `../legacy/config/schema.sql`.)*

### Step 6: Start the Local Development Server

You can run the application using either the Symfony CLI or PHP's built-in web server.

**Option A: Using Symfony CLI (Recommended)**
```bash
symfony server:start
```
Or run it in the background:
```bash
symfony serve -d
```

**Option B: Using PHP Built-in Web Server**
```bash
php -S localhost:8000 -t public
```

Once the server is running, access the application at:
- **Public Storefront**: `http://localhost:8000/`
- **Admin Dashboard**: `http://localhost:8000/dashboard`

---

## Application Routes and Endpoints

You can inspect all registered routes at any time by running `php bin/console debug:router`. Below is the complete reference of available endpoints:

### Public Storefront Routes

| Method | URI Path | Route Name | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/` | `app_home` | Main landing page with featured prices and testimonials |
| `GET` | `/home` | `app_home_redirect` | 301 permanent redirect to `/` |
| `GET, POST` | `/services` | `app_services` | Full service catalog and quick appointment submission |
| `GET` | `/about` | `app_about` | Salon background and team member profiles |
| `GET, POST` | `/contact` | `app_contact` | Customer inquiry contact form |
| `GET, POST` | `/booking` | `app_booking` | Interactive appointment reservation form |
| `GET` | `/api/slots` | `app_api_slots` | JSON endpoint returning available time slots (`?staff_id=1&date=YYYY-MM-DD&service_id=1`) |
| `GET, POST` | `/logout` | `app_logout` | Invalidates the current user session and redirects home |

### Administrative Dashboard Routes

| Method | URI Path | Route Name | Description |
| :--- | :--- | :--- | :--- |
| `GET, POST` | `/dashboard` | `app_dashboard` | Main administrative overview and KPI summary |
| `GET` | `/dashboard/appointments` | `admin_appointments` | Appointment listing and filtering view |
| `GET` | `/dashboard/services` | `admin_services` | Service catalog management view |
| `GET` | `/dashboard/customers` | `admin_customers` | Registered customer directory |
| `GET` | `/dashboard/users` | `admin_users` | System user account management view |
| `GET` | `/dashboard/activity-logs` | `admin_activity_logs` | System audit and booking event log view |
| `GET` | `/dashboard/kpi-stats` | `admin_kpi_stats` | JSON endpoint returning real-time dashboard counts |
| `POST` | `/dashboard/appointment/status` | `admin_appointment_status` | Updates an appointment's status |
| `POST` | `/dashboard/appointment/update` | `admin_appointment_update` | Updates appointment staff, date, time, service, and price |
| `POST` | `/dashboard/appointment/delete` | `admin_appointment_delete` | Deletes an appointment record |
| `POST` | `/dashboard/service/create` | `admin_service_create` | Adds a new treatment to the service catalog |
| `POST` | `/dashboard/service/update` | `admin_service_update` | Updates an existing service's details and pricing |
| `POST` | `/dashboard/service/delete` | `admin_service_delete` | Removes a service from the catalog |
| `POST` | `/dashboard/user/create` | `admin_user_create` | Creates a new user account |
| `POST` | `/dashboard/user/update` | `admin_user_update` | Updates user role and active status |
| `POST` | `/dashboard/user/delete` | `admin_user_delete` | Deletes a user account |

---

## Running Automated Tests

The project uses PHPUnit (`WebTestCase`) for functional testing of controllers, templates, and layout constraints.

To execute the test suite via the terminal in headless mode:

```bash
php bin/phpunit
```

### Useful Diagnostic and Linting Commands

Before deploying or committing changes, validate the container, Twig templates, and YAML configurations:

```bash
# Clear the application cache
php bin/console cache:clear

# Validate dependency injection service wiring
php bin/console lint:container

# Validate all Twig templates for syntax errors
php bin/console lint:twig templates/

# Validate YAML configuration files
php bin/console lint:yaml config/
```

---

## Troubleshooting

### 1. PHP Version Mismatch During `composer install`
- **Issue**: Composer reports that your PHP version does not satisfy `>=8.4`.
- **Solution**: Verify your CLI PHP version with `php -v`. Ensure PHP 8.4 or newer is installed and added to your system `PATH`.

### 2. Missing Styles or JavaScript Errors on Load
- **Issue**: The storefront or dashboard loads without styling or Stimulus controllers fail to initialize.
- **Solution**: Run the asset and importmap installation commands and clear the cache:
  ```bash
  php bin/console importmap:install
  php bin/console assets:install public
  php bin/console cache:clear
  ```

### 3. Invalid CSRF Token Error When Submitting Forms
- **Issue**: Booking or dashboard management forms return an "Invalid security token" or 403 Access Denied response.
- **Solution**: Ensure cookies and PHP sessions are enabled in your browser and that `var/cache` and `var/share` are writable by the PHP process.

### 4. Port 8000 is Already in Use
- **Issue**: Starting the local server fails because port 8000 is occupied.
- **Solution**: Specify an alternative port when starting the server, for example:
  ```bash
  php -S localhost:8080 -t public
  ```

---

## License

This project is proprietary software developed for TRUNO's Beauty Salon (S Beauty Salon). All rights reserved.
