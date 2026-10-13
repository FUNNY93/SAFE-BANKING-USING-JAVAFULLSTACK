# SAFE BANK — Online Banking Management System

SAFE BANK is a full-stack banking application built with a React frontend and a Java Spring Boot backend. It demonstrates an account-application workflow, user and administrator pages, balance deposits, money transfers, transaction history, OTP-based login, and transaction-statement generation.

> **Project status:** This repository is a learning/demo project, not a production banking platform. Do not use real customer data, real banking credentials, or real money. Review the security checklist before making the repository public.

## Table of Contents

- [Overview](#overview)
- [Features Present in the Source Code](#features-present-in-the-source-code)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Repository Structure](#repository-structure)
- [Screenshots](#screenshots)
- [Prerequisites](#prerequisites)
- [Setup and Run](#setup-and-run)
- [Configuration](#configuration)
- [API Reference](#api-reference)
- [Authentication and Security Notes](#authentication-and-security-notes)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [Possible Future Improvements](#possible-future-improvements)
- [Learning Outcomes](#learning-outcomes)
- [Author](#author)
- [License](#license)

## Overview

SAFE BANK provides a web interface for a simulated online banking workflow. The React application communicates with REST endpoints implemented in Spring Boot. User and transaction data are persisted through Spring Data JPA/Hibernate with MySQL.

The source code includes account-application submission and approval flows, OTP login endpoints, deposit and transfer operations, transaction listing, profile/settings pages, a chatbot UI connected to OpenRouter, and PDF transaction-statement generation with email support.

This project is intended for demonstration and learning. It has not been audited or validated for real financial use.

## Features Present in the Source Code

The following capabilities are represented in the provided source code. Their end-to-end behaviour should be tested in a clean environment before describing them as production-ready.

- Account application form with personal, KYC, and account details.
- Application status, review, and success pages.
- User login flow using email OTP verification endpoints and JWT token handling in the frontend.
- Dashboard, profile, settings, and contact pages.
- Deposit and money-transfer API operations.
- Transaction history and transaction CRUD endpoints.
- Admin login and account approval/rejection pages and endpoints.
- PIN-change endpoint.
- PDF transaction-statement generation and email integration.
- Light/dark theme styling.
- SAFE BANK chatbot interface using an OpenRouter API call.
- Internationalization setup using i18next.
- Google OAuth client configuration is present in backend properties; complete end-to-end OAuth behaviour has not been verified.

## Technology Stack

| Area | Technologies found in the project |
|---|---|
| Frontend | React 19, JavaScript, Vite 8, React Router, Axios, CSS |
| Frontend supporting libraries | React Icons, i18next, react-i18next, Firebase SDK |
| AI assistant | OpenRouter API integration in `src/services/openrouter.js` |
| Backend | Java 17, Spring Boot 3.5.4, Spring Web, Spring Security |
| Persistence | Spring Data JPA, Hibernate, MySQL Connector/J |
| Authentication components | JWT library (JJWT), OTP endpoints, Spring Security |
| Email and documents | Spring Boot Mail, Apache PDFBox, Thymeleaf dependency |
| Build tools | npm, Maven / Maven Wrapper |
| Testing dependency | Spring Boot Test / JUnit 5 |

The uploaded backend archive contains a second nested `banking/` Maven project as well as the root Maven project. The root project contains the full application code and should be treated as the intended backend unless you confirm otherwise. Remove or consolidate the duplicate project before publishing to avoid confusion.

## Architecture

```text
Browser
  |
  | React UI / React Router
  v
Vite development server (default: http://localhost:5173)
  |
  | HTTP REST requests
  v
Spring Boot API (configured: http://localhost:8082)
  |
  +--> Spring Security / JWT filter
  |
  +--> Service layer
  |      +--> User management
  |      +--> Deposit and transfer logic
  |      +--> Statement generation / email
  |
  +--> Spring Data JPA / Hibernate
             |
             v
           MySQL

Frontend chatbot --> OpenRouter API (requires VITE_OPENROUTER_API_KEY)
```

## Repository Structure

After tidying the supplied archives, a recommended single-repository layout is:

```text
SAFE-BANK/
├── README.md
├── .gitignore
├── frontend/
│   ├── package.json
│   ├── package-lock.json
│   ├── vite.config.js
│   ├── public/
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── services/
│       └── css/
├── backend/
│   ├── pom.xml
│   └── src/
│       ├── main/
│       │   ├── java/com/safebank/banking/
│       │   └── resources/application.properties
│       └── test/
└── docs/
    └── images/
        ├── welcome-page.png
        ├── login-page.png
        ├── account-application.png
        ├── dashboard.png
        ├── send-money.png
        └── transaction-history.png
```

The screenshot names above are suggestions, not files included in the uploaded archives. Add your real screenshots before relying on these image links.

## Screenshots

Add screenshots of the actual running application to `docs/images/`. Do not include real personal, KYC, account, OTP, or transaction information in screenshots.

Once the files are added, replace or retain the relevant entries below:

### Welcome Page

![SAFE BANK welcome page](docs/images/welcome-page.png)

### Login Page

![SAFE BANK login page](docs/images/login-page.png)

### Account Application

![SAFE BANK account application](docs/images/account-application.png)

### Dashboard

![SAFE BANK dashboard](docs/images/dashboard.png)

### Send Money

![SAFE BANK send money page](docs/images/send-money.png)

### Transaction History

![SAFE BANK transaction history](docs/images/transaction-history.png)

If a screenshot is not available, remove its image section until the file is added. GitHub will show a broken image for a path that does not exist.

## Prerequisites

Install the following software:

- Java Development Kit (JDK) 17.
- Node.js and npm, compatible with the Vite version in `frontend/package.json`.
- MySQL Server.
- Git.
- A Google OAuth client only if you intend to configure and use the OAuth flow.
- An OpenRouter API key only if you intend to use the AI chatbot.

The backend's configured port is `8082`. The frontend uses Vite's default development port, `5173`.

## Setup and Run

### 1. Clone the repository

Replace the example URL with your actual GitHub repository URL:

```bash
git clone https://github.com/YOUR-USERNAME/SAFE-BANK.git
cd SAFE-BANK
```

### 2. Create the MySQL database

Create a local database in MySQL. For example:

```sql
CREATE DATABASE safebank;
```

The database name and connection URL must match your environment variables. The backend uses `spring.jpa.hibernate.ddl-auto=update`, which lets Hibernate update mapped schema during development. For a serious deployment, use managed database migrations and review schema changes rather than relying on automatic updates.

### 3. Configure the backend

Open a terminal in the `backend/` directory. Set the environment variables listed in [Configuration](#configuration) before starting the application.

On Windows PowerShell, for the current terminal session, use values appropriate to your local environment:

```powershell
$env:DB_URL="jdbc:mysql://localhost:3306/safebank"
$env:DB_USERNAME="your_mysql_username"
$env:DB_PASSWORD="your_mysql_password"
$env:JWT_SECRET="replace_with_a_long_random_secret"
$env:MAIL_USERNAME="your_test_email"
$env:MAIL_PASSWORD="your_email_app_password"
$env:GOOGLE_CLIENT_ID="your_google_oauth_client_id"
$env:GOOGLE_CLIENT_SECRET="your_google_oauth_client_secret"
```

If you use a feature that requires a particular integration, configure its variables. Never commit actual values.

Start the backend with the Maven Wrapper:

**Windows:**
```powershell
.\mvnw.cmd spring-boot:run
```

**macOS/Linux:**
```bash
./mvnw spring-boot:run
```

Run these commands from the directory containing the intended backend `pom.xml`. If the wrapper is not executable on macOS/Linux, run `chmod +x mvnw` first.

The API should start on `http://localhost:8082` if the configured port is available and all required configuration is valid. A successful startup does not mean every endpoint or integration has been tested.

### 4. Configure and run the frontend

Open a second terminal in the `frontend/` directory.

Install dependencies from the lockfile:

```bash
npm ci
```

Create a local `.env` file if you want to enable the chatbot:

```dotenv
VITE_OPENROUTER_API_KEY=your_openrouter_api_key
```

Start the development server:

```bash
npm run dev
```

Open the URL printed by Vite, normally `http://localhost:5173`.

The current frontend source contains several hard-coded API URLs pointing to `http://localhost:8082`. If you change the backend address or deploy the app elsewhere, update these references or refactor them to use a Vite environment variable such as `VITE_API_BASE_URL`.

### 5. Verify the connection

1. Confirm MySQL is running and the database credentials are correct.
2. Confirm the Spring Boot process starts without configuration errors.
3. Open the frontend at the Vite URL.
4. Test account application submission and check the backend logs.
5. Test OTP delivery only after configuring mail.
6. Test deposits, transfers, profile changes, and transaction history using dummy data only.
7. Check browser Developer Tools → Network and Console for failed requests.
8. Check authorization behaviour carefully; the frontend must send the expected `Authorization: Bearer <token>` header for protected endpoints.

## Configuration

The backend `application.properties` expects the following environment variables:

| Variable | Purpose | Required for |
|---|---|---|
| `DB_URL` | JDBC URL, e.g. `jdbc:mysql://localhost:3306/safebank` | Database access |
| `DB_USERNAME` | MySQL username | Database access |
| `DB_PASSWORD` | MySQL password | Database access |
| `JWT_SECRET` | Secret used by JWT implementation | JWT token operations |
| `MAIL_USERNAME` | SMTP account username | Email / OTP / statement email |
| `MAIL_PASSWORD` | SMTP credential or app password | Email / OTP / statement email |
| `GOOGLE_CLIENT_ID` | Google OAuth client ID | Google OAuth, if enabled |
| `GOOGLE_CLIENT_SECRET` | Google OAuth client secret | Google OAuth, if enabled |
| `VITE_OPENROUTER_API_KEY` | OpenRouter API key | Frontend chatbot |

The backend also configures SMTP through `smtp.gmail.com:587` and uses port `8082`.

**Important configuration notes:**

- The backend properties include `spring.jpa.show-sql=true` and verbose Spring Security logging. Disable or reduce these for shared/public environments.
- Check the whitespace before `jwt.secret=${JWT_SECRET}` in `application.properties`; property names should be cleanly formatted and the JWT configuration should be tested at startup.
- Do not place real credentials in the README, source code, screenshots, issue reports, or commit history.
- Frontend variables prefixed with `VITE_` are bundled into client-side code. Do not treat them as private server-side secrets. If an API provider supports restrictions, configure them appropriately; proxy sensitive API calls through a backend when necessary.
- The Firebase configuration in `src/firebase.js` contains a project API key and identifiers. Review Firebase security rules and API-key restrictions before publishing. Firebase web API keys are commonly client-visible identifiers, but they are not a substitute for access controls.

## API Reference

Base URL for local development: `http://localhost:8082`

The following routes are derived from the controller annotations in the supplied backend. Role requirements come from the current `SecurityConfig`; actual request success depends on configuration and valid authentication.

### Authentication

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/auth/send-otp` | Request an email OTP |
| `POST` | `/auth/verify-otp` | Verify OTP and, according to the frontend flow, receive a JWT and user details |
| `POST` | `/admin/login` | Admin login |

### Users

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/users` | Submit a new account application |
| `GET` | `/users` | List users |
| `GET` | `/users/{id}` | Get a user by ID |
| `PUT` | `/users/{id}` | Update user profile fields |
| `PUT` | `/users/{id}/approve` | Approve an application |
| `PUT` | `/users/{id}/reject` | Reject an application |
| `PUT` | `/users/{id}/pin` | Change a user's PIN |
| `DELETE` | `/users/{id}` | Delete a user |
| `GET` | `/users/mobile/{mobile}` | Look up a user by mobile number |
| `GET` | `/users/email/{email}` | Look up a user by email |

### Transactions

| Method | Endpoint | Purpose |
|---|---|---|
| `POST` | `/transactions` | Create a transaction record |
| `GET` | `/transactions` | List transactions |
| `GET` | `/transactions/{id}` | Get a transaction by ID |
| `PUT` | `/transactions/{id}` | Update a transaction record |
| `DELETE` | `/transactions/{id}` | Delete a transaction record |
| `POST` | `/transactions/send` | Transfer funds between accounts |
| `POST` | `/transactions/deposit` | Deposit funds into an account |
| `GET` | `/transactions/statement/{accountNumber}` | Generate a transaction statement and trigger the configured statement flow |

The endpoint list is a route inventory, not a claim that the API has complete OpenAPI/Swagger documentation. Request bodies and response formats should be documented from controller/service behaviour and verified with tests before adding detailed public examples.

## Authentication and Security Notes

The backend contains Spring Security configuration, a JWT authentication filter, role checks for admin/user routes, OTP endpoints, and Google OAuth client properties. However, the presence of security components does **not** establish that the application is secure for real banking use.

Before publishing or using this project with anything other than synthetic data, address these findings from the source review:

1. **Sensitive fields in API responses:** The `User` entity contains PIN, Aadhaar, PAN, contact details, and other personal fields. Several controller methods return entity objects directly. Use dedicated response DTOs that omit secrets and sensitive identifiers, and mask sensitive values.
2. **PIN storage and handling:** The entity stores a `pin` field, and the PIN-change service compares the supplied PIN directly. Do not store PINs in plaintext. Use a suitable password-hashing mechanism and enforce rate limiting, lockout, and authorization checks.
3. **Debug logging:** The JWT filter logs the `Authorization` header and token-related details. Remove token/header logging. Review other console logs for personal data, OTPs, PINs, passwords, email addresses, and KYC details.
4. **Frontend logs:** The account-review page logs the submitted application payload, which includes sensitive KYC and account information. Remove these logs.
5. **Authorization headers:** The frontend login flow stores a JWT in local storage, but inspected API calls do not consistently attach it as a bearer token. Add and test a shared API client/interceptor and verify that protected calls work only for authorized users.
6. **Endpoint authorization:** Review the authorization rules for every user, admin, and transaction endpoint. Ensure users can only access their own records and transactions. Avoid allowing a client to submit arbitrary account numbers or modify transaction records without ownership checks.
7. **CORS:** Multiple CORS configurations exist, including permissive controller-level origins. Consolidate them and allow only the intended origins for the target environment.
8. **Secret management:** Keep database, SMTP, OAuth, JWT, and AI-provider credentials out of Git. If any real secret was committed previously, rotate it; deleting it from the latest version alone does not remove it from Git history.
9. **Database settings:** Disable SQL and security debug logging in shared environments. Use least-privilege database credentials and review schema migration strategy.
10. **Financial correctness:** Add rigorous validation, idempotency, concurrency tests, audit trails, and reconciliation controls for monetary operations. This demo has not been independently audited.

Do not represent this application as a secure production bank or connect it to real financial accounts.

## Testing

The backend archive contains a basic Spring Boot context-load test. No comprehensive automated tests for authentication, access control, account ownership, deposits, transfers, or statement generation were identified during this documentation review.

Run the backend test suite from the backend directory:

```bash
./mvnw test
```

On Windows:

```powershell
.\mvnw.cmd test
```

Run frontend lint/build checks from the frontend directory:

```bash
npm run lint
npm run build
```

These commands are provided for verification; test and build success have not been asserted here. Add unit, integration, and end-to-end tests and document the results after running them.

## Troubleshooting

| Problem | Checks |
|---|---|
| Frontend cannot reach backend | Confirm Spring Boot is running on port `8082`, then inspect browser Network/Console and backend logs. |
| `403 Forbidden` from a protected endpoint | Check the security rules and whether the request includes a valid bearer JWT with the expected role. |
| Database connection failure | Verify MySQL is running and `DB_URL`, `DB_USERNAME`, and `DB_PASSWORD` are set correctly. |
| OTP/email is not delivered | Verify SMTP credentials, Gmail app-password configuration if applicable, and network access to SMTP. Never log OTPs or passwords. |
| Chatbot cannot respond | Verify `VITE_OPENROUTER_API_KEY` is set, restart Vite after changing `.env`, and check provider/network errors. Remember that Vite variables are exposed to browser code. |
| Screenshot is broken on GitHub | Check that the image is committed and the Markdown path matches the filename and letter case. |
| Port already in use | Stop the process using the port or configure a different port and update the frontend API URL/CORS settings. |

## Possible Future Improvements

These are recommendations, not claims that the features are already implemented:

- Use DTOs and validation for all API requests/responses.
- Hash PINs and implement secure credential and OTP lifecycle controls.
- Enforce per-user ownership checks for accounts and transactions.
- Centralize API URLs in environment configuration.
- Add API documentation with OpenAPI/Swagger.
- Add unit, integration, security, and end-to-end tests.
- Add database migrations, audit logs, and structured logging with sensitive-field redaction.
- Consolidate duplicate backend project files and CORS configuration.
- Improve responsive accessibility and loading/error states.
- Deploy to a secure hosting environment only after a security review.

## Learning Outcomes

This project provides practice with:

- Building a React single-page application with routing and reusable components.
- Integrating a frontend with REST APIs.
- Developing Spring Boot controllers, services, entities, and repositories.
- Connecting Java applications to MySQL using JPA/Hibernate.
- Exploring JWT-based authentication, role-based authorization, and OTP flows.
- Implementing simulated deposits/transfers and transaction history.
- Generating PDF statements and integrating email services.
- Managing frontend/backend configuration and preparing project documentation.

## Author

**Author:** Add your preferred public name here  
**GitHub:** Add your GitHub profile URL  
**LinkedIn:** Add your LinkedIn profile URL

Only include contact details you want to publish publicly.

## License

No license file was present in the supplied project archive. Choose and add a license (for example, MIT) only if you intend to grant those permissions. Until a license is added, do not assume others have permission to reuse or redistribute the code.
