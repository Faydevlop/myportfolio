# Sainik Setu – Backend Service

Node.js (TypeScript) API that powers the Sainik Setu platform. It exposes admin and client endpoints for managing services, enquiries, members, content, authentication workflows, storage operations, and dashboards, backed by MongoDB and S3-compatible storage.

## Tech Stack

- Node.js 18 / Express 4
- TypeScript 5
- MongoDB & Mongoose
- AWS S3 (or compatible storage) for media
- Resend email service
- Swagger UI for API documentation

## Prerequisites

- Node.js ≥ 18 and npm
- MongoDB instance reachable from the service
- AWS S3 (or MinIO) bucket and credentials
- Resend account (or configure another provider in code)
- Optional: access to Google, Apple, and Facebook OAuth apps for social login

## Getting Started

1. Clone the repository and install dependencies:
   ```bash
   git clone <repo-url>
   cd sainik-setu-bk-service
   npm install
   ```
2. Copy the sample environment file and fill in secrets:
   ```bash
   cp sample.env .env
   # update values in .env
   ```
3. Start the development server with hot reload:
   ```bash
   npm run start
   ```
4. Build and run in production mode:
   ```bash
   npm run build
   node lib/index.js
   ```

### Useful Scripts

- `npm run lint` – fix lint issues with ESLint
- `npm run format` – format sources with Prettier
- `npm test` – execute Jest tests (add tests before relying on this)
- `npm run seed` – seed core data (requires scripts and env to be configured)

## Environment Variables

The `sample.env` file lists every variable consumed by the service, grouped by concern (core app, database, email, storage, OAuth, etc.). Copy it to `.env` and adjust:

- Mongo connections: `MONGODB_URI`, `DEMO_MONGODB_URI`
- Auth: `JWT_SECRET_KEY`, token expiries, `OTP_EXPIRY`
- Storage: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `S3_BUCKET`, `S3_ENDPOINT`, `S3_PORT`, `S3_SSL`
- Email: `RESEND_API_KEY`, `RESEND_FROM`, `SERVICE_PROVIDER`
- Admin seed user: `SEED.SUPER_ADMIN.NAME|EMAIL|PASSWORD`
- OAuth: `GOOGLE_...`, `APPLE_...`, `FACEBOOK_...`
- Misc: `CORS_URLS`, `SWAGGER_URLS`, `BK_SERVICE_URL`, `LOG_LEVEL`

Any missing mandatory variable will cause the service to crash on startup, so ensure `.env` is complete before running.

## API Documentation

- Swagger UI: `GET /v1/swagger` (useful during development to explore schemas and try requests)
- Postman: import the Swagger JSON from `GET /v1/swagger-json` (exposed via Swagger UI)

Authentication guard differs by route group:

- Client routes typically require `Authorization: Bearer <access_token>` after social login.
- Admin routes require admin bearer tokens obtained from `/v1/admin/login`.
- Some public endpoints (blogs/services listing, contact form, constants) are open by default.

## Route Overview

### Generic Utilities (`/v1/generic`)

| Method | Path                     | Purpose                                               |
| ------ | ------------------------ | ----------------------------------------------------- |
| GET    | `/v1/generic/constants`  | Fetch constant dictionaries (collections, roles, etc.) |
| POST   | `/v1/generic/image`      | Generate a presigned POST for uploading to storage    |
| POST   | `/v1/generic/getImage`   | Return signed GET URLs for stored assets              |
| POST   | `/v1/generic/location`   | Resolve Indian states or districts                    |

### Client Authentication & Profile (`/v1/client`)

| Method | Path                        | Purpose                                                      | Notes                     |
| ------ | --------------------------- | ------------------------------------------------------------ | ------------------------- |
| POST   | `/v1/client/register`       | Register/login via Google, Apple, or Facebook tokens         | Requires provider + token |
| POST   | `/v1/client/updateProfile`  | Create or update user profile with address and proof docs    | Requires bearer token     |
| POST   | `/v1/client/refresh`        | Issue new access/refresh tokens                              | Requires refresh token    |
| POST   | `/v1/client/logout`         | Revoke refresh token and end session                         | Requires bearer token     |
| GET    | `/v1/client/getProfile`     | Retrieve the authenticated client profile                    | Requires bearer token     |

### Client Content (`/v1/client`)

| Method | Path                           | Purpose                                              | Notes             |
| ------ | ------------------------------ | ---------------------------------------------------- | ----------------- |
| POST   | `/v1/client/getAllBlogs`       | List blogs with filters, search, pagination          | Public            |
| GET    | `/v1/client/getOneBlog/:slug`  | Fetch a single blog post by slug                     | Public            |
| POST   | `/v1/client/getAllServices`    | List services with filters, search, pagination       | Public            |
| GET    | `/v1/client/getOneService/:slug` | Retrieve service details by slug                   | Public            |
| POST   | `/v1/client/contactUs`         | Submit an enquiry/contact request                    | Public            |
| POST   | `/v1/client/getEnquiries`      | List enquiries created by the authenticated client   | Requires bearer token |

### Admin Authentication (`/v1/admin`)

| Method | Path                         | Purpose                                   |
| ------ | ---------------------------- | ----------------------------------------- |
| POST   | `/v1/admin/login`            | Log in seeded admin user                  |
| POST   | `/v1/admin/sendOTP`          | Send OTP for password reset               |
| POST   | `/v1/admin/verifyOTP`        | Validate OTP during password reset        |
| POST   | `/v1/admin/reset-password`   | Reset admin password                      |
| POST   | `/v1/admin/refresh`          | Refresh admin access token                |
| POST   | `/v1/admin/logout`           | Invalidate refresh token and logout       |

### Admin Content & Management (`/v1/admin`)

| Method | Path                              | Purpose                                         |
| ------ | --------------------------------- | ----------------------------------------------- |
| POST   | `/v1/admin/addBlog`               | Create blog content                             |
| POST   | `/v1/admin/getAllBlogs`           | List blogs with filters/paging                  |
| PUT    | `/v1/admin/editBlog/:id`          | Update blog content                             |
| GET    | `/v1/admin/getOneBlog/:id`        | Retrieve a specific blog by ID                  |
| DELETE | `/v1/admin/deleteBlog/:id`        | Remove blog entry                               |
| POST   | `/v1/admin/addService`            | Create service listing                          |
| POST   | `/v1/admin/getAllServices`        | List services with filters/paging               |
| PUT    | `/v1/admin/editService/:id`       | Update service details                          |
| GET    | `/v1/admin/getOneService/:id`     | Retrieve service by ID                          |
| DELETE | `/v1/admin/deleteService/:id`     | Remove a service listing                        |
| POST   | `/v1/admin/getAllEnquiries`       | Review enquiries with filters/paging            |
| GET    | `/v1/admin/getOneEnquiry/:id`     | View enquiry details                            |
| DELETE | `/v1/admin/deleteEnquiry/:id`     | Delete enquiry record                           |
| POST   | `/v1/admin/getAllMembers`         | List registered members                         |
| GET    | `/v1/admin/getOneMember/:id`      | View member details                             |
| DELETE | `/v1/admin/deleteMember/:id`      | Remove member                                   |
| POST   | `/v1/admin/getAllRequests`        | List service requests                            |
| GET    | `/v1/admin/getOneRequest/:id`     | View service request details                    |
| PUT    | `/v1/admin/updateRequest/:id`     | Update request status/details                   |
| DELETE | `/v1/admin/deleteRequest/:id`     | Delete request                                  |
| GET    | `/v1/admin/dashboard/stats`       | Aggregated dashboard counters                   |
| POST   | `/v1/admin/dashboard/logs`        | Retrieve dashboard log entries (filters/paging) |



