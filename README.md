# Softwraith Platform — Complete Developer Documentation

Softwraith is a premium MERN-style company website, LMS (Learning Management System), and Admin Management platform. This document provides setup instructions, environment configurations, API endpoint references, and deployment notes for developers.

## Project Structure

```text
softwraith/
├── backend/                  # Express API, MongoDB models, controllers, routes
│   ├── config/               # Database and configuration files
│   ├── controllers/          # Business logic handlers
│   ├── middlewares/          # Auth, security, and input validators
│   ├── models/               # Mongoose schemas
│   ├── routes/               # API route definitions
│   ├── scripts/              # Database seed and helper scripts
│   ├── tests/                # Jest & Supertest integration test suite
│   └── utils/                # PDF generation and mail templates
└── frontend/                 # React, Vite, and custom CSS client
    ├── src/
    │   ├── components/       # Layouts, Navbar, and UI widgets (Spinner, States)
    │   ├── context/          # Auth state provider
    │   ├── pages/            # Core views (Home, About, Services, Course detail)
    │   └── utils/            # Axios interceptor instance
```

## Local Setup

### 1. Prerequisites
- **Node.js**: v18+
- **MongoDB**: A local instance or a MongoDB Atlas connection string.

### 2. Dependency Installation
Run the following commands to install dependencies for both components:
```bash
# Install backend packages
cd backend
npm install

# Install frontend packages
cd ../frontend
npm install
```

### 3. Environment Variable Files
Create `.env` files from their respective examples:
```bash
# In backend/
cp .env.example .env

# In frontend/
cp .env.example .env
```

### 4. Database Seeding
To populate the database with dummy admin, student users, free, and paid courses, execute:
```bash
cd backend
npm run seed
```

### 5. Running the Application
Start the development servers concurrently:
```bash
# In backend/
npm run dev

# In frontend/
npm run dev
```
- **Frontend URL**: `http://localhost:5173`
- **Backend API URL**: `http://localhost:5000`

---

## Environment Variables Reference

### Backend (`backend/.env`)

| Variable | Description | Example / Default |
|----------|-------------|-------------------|
| `PORT` | The port the Express API listens on. | `5000` |
| `NODE_ENV` | Environment mode (`development` or `production`). | `development` |
| `MONGO_URI` | MongoDB Connection URI. | `mongodb://127.0.0.1:27017/softwraith` |
| `JWT_SECRET` | Secret key used to sign JWT authentication tokens. | `some-random-long-secret-key` |
| `CLIENT_URL` | Frontend origin URL for CORS policy. | `http://localhost:5173` |
| `GOOGLE_CLIENT_ID` | Google OAuth Client ID for safe-mode auth. | `optional-google-client-id` |
| `SMTP_HOST` | SMTP server host for sending password reset emails. | `smtp.gmail.com` |
| `SMTP_PORT` | SMTP server port. | `587` |
| `SMTP_USER` | SMTP server username. | `noreply@softwraith.com` |
| `SMTP_PASS` | SMTP server password. | `your-app-password` |
| `FROM_EMAIL` | From address in outbound emails. | `noreply@softwraith.com` |
| `RAZORPAY_KEY_ID` | Razorpay Key ID (Payment simulation active if omitted). | `rzp_test_xxxx` |
| `RAZORPAY_KEY_SECRET` | Razorpay API Secret key. | `xxxx` |

### Frontend (`frontend/.env`)

| Variable | Description | Example / Default |
|----------|-------------|-------------------|
| `VITE_API_URL` | Base API target for Axios calls. | `http://localhost:5000/api` |
| `VITE_GOOGLE_CLIENT_ID`| Client ID for Google login UI wrapper. | `optional-google-client-id` |

---

## Testing & Quality Control

### Running Backend Tests
The test suite utilizes Jest & Supertest to run mock-isolated queries. The test database is automatically created, populated, and dropped at the end of execution.
```bash
cd backend
npm test
```

---

## API Endpoints List

### Authentication
- `POST /api/auth/register` - Create user
- `POST /api/auth/login` - Authenticate user
- `GET /api/auth/me` - Profile recovery (Protected)
- `PUT /api/auth/password` - Change password (Protected)
- `POST /api/auth/forgot-password` - Generate reset token and email
- `POST /api/auth/reset-password/:token` - Update password via token

### Courses & Curriculum
- `GET /api/courses` - List all published courses
- `GET /api/courses/:id` - View detailed course details
- `POST /api/courses` - Create new course (Admin only)
- `PUT /api/courses/:id` - Update course parameters (Admin only)
- `DELETE /api/courses/:id` - Cascade delete course, lectures, & enrollments (Admin only)
- `GET /api/lectures/:courseId` - Retrieve course curriculum lectures
- `POST /api/lectures` - Add lecture (Admin only)
- `PUT /api/lectures/:id` - Modify lecture parameters (Admin only)
- `DELETE /api/lectures/:id` - Delete lecture (Admin only)

### Enrollments & Learning Progress
- `POST /api/enroll` - Enroll in free course (Protected)
- `GET /api/enroll/mycourses` - List active learning enrollments (Protected)
- `GET /api/enroll/progress/:enrollmentId` - Retrieve course completion stats (Protected)
- `POST /api/enroll/progress/:enrollmentId/complete/:lectureId` - Mark lecture complete (Protected)
- `DELETE /api/enroll/progress/:enrollmentId/complete/:lectureId` - Mark lecture incomplete (Protected)

### Certificates
- `POST /api/certificates/generate/:enrollmentId` - Issue certificate PDF (Protected)
- `GET /api/certificates/my` - List earned certificates (Protected)
- `GET /api/certificates/download/:id` - Download generated certificate PDF (Protected)
- `GET /api/certificates/verify/:certificateNumber` - Public certificate verification

### Contact & Services Requests
- `POST /api/contact` - Submit support enquiry
- `GET /api/contact` - Read inbound enquiries (Admin only)
- `POST /api/services` - Create custom service request (Protected)
- `GET /api/services/my` - List user's service requests (Protected)
- `GET /api/services` - View all service requests (Admin only)

### Payments
- `POST /api/payments/create-order/:courseId` - Initiate order (Protected)
- `POST /api/payments/verify` - Verify signature & activate enrollment (Protected)
- `GET /api/payments/history` - User transaction list (Protected)
- `GET /api/payments/admin/all` - Global payment logging (Admin only)
