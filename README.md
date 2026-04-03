

# Online Course Platform - Microservices Architecture

> A comprehensive online course platform built with a microservices architecture, allowing users to learn new skills, enroll in courses, track progress, and earn certificates.

##  Table of Contents
- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Services](#services)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Running the Project](#running-the-project)
- [API Endpoints](#api-endpoints)
- [Project Structure](#project-structure)

---

##  Project Overview

The Online Course Platform is a full-stack application designed with microservices architecture to provide a scalable, maintainable, and independent set of services for managing an online learning platform.

**Key Features:**
- User authentication and authorization
- Course management and browsing
- Student enrollment system
- Progress tracking and monitoring
- Certificate generation upon completion
- API Gateway for unified access

---

##  Architecture

This project follows a **Microservices Architecture** with the following components:

```
┌─────────────────────────────────────────────────────────────────┐
│                    Frontend (React + Vite)                      │
│                      (Port 3000)                                │
└──────────────────────────┬──────────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────────┐
│                      API Gateway                                 │
│                   (Port 4000)                                    │
│        Routes requests to appropriate microservices             │
└──────────┬──────────┬──────────┬──────────┬────────────────────┘
           │          │          │          │
    ┌──────▼───┐ ┌────▼────┐ ┌──┴─────┐ ┌──┴────────┐
    │  User    │ │ Course  │ │Enroll  │ │ Progress │ ┌──────────┐
    │ Service  │ │ Service │ │Service │ │ Service  │ │Cert      │
    │(5000)    │ │(5001)   │ │(5002)  │ │ (5003)   │ │Service   │
    └───┬──────┘ └────┬────┘ └──┬─────┘ └──┬───────┘ │  (5004)  │
        │             │          │          │        └──────────┘
        └─────────────┴──────────┴──────────┴─────────────┐
                                                          │
                                                    ┌─────▼──────┐
                                                    │  MongoDB   │
                                                    │ (27017)    │
                                                    └────────────┘
```

---

##  Services

### 1. **API Gateway** (Port 4000)
- Central entry point for all client requests
- Routes requests to appropriate microservices
- Handles CORS and basic request forwarding
- `Location: /api-gateway`

### 2. **User Service** (Port 5000)
- User registration and authentication
- User profile management
- JWT token generation
- `Location: /user-service`
- **Database:** MongoDB Collection - `users`

### 3. **Course Service** (Port 5001)
- Course creation and management
- Course listing and filtering
- Course details and module information
- `Location: /course-service`
- **Database:** MongoDB Collection - `courses`

### 4. **Enrollment Service** (Port 5002)
- Student course enrollment
- Enrollment status management
- Enrollment history tracking
- `Location: /enrollment-service`
- **Database:** MongoDB Collection - `enrollments`

### 5. **Progress Tracking Service** (Port 5003)
- Track student progress per course
- Lesson completion tracking
- Progress percentage calculation
- Course statistics and analytics
- `Location: /progress-service`
- **Database:** MongoDB Collection - `progress`

### 6. **Certificate Service** (Port 5004)
- Certificate generation upon course completion
- Certificate verification
- Certificate revocation
- Skill tracking
- `Location: /certificate-service`
- **Database:** MongoDB Collection - `certificates`

### 7. **Frontend** (Port 3000)
- React + Vite application
- User interface for all platform features
- Authentication and dashboard
- `Location: /frontend`

---

##  Prerequisites

- **Node.js** (v14 or higher)
- **MongoDB** (v5.0 or higher)
- **Docker & Docker Compose** (optional, for containerized deployment)
- **npm** or **yarn**

---

##  Installation

### 1. Clone the repository
```bash
cd "Online Course Platform"
```

### 2. Install dependencies for each service

#### Frontend
```bash
cd frontend
npm install
```

#### API Gateway
```bash
cd api-gateway
npm install
```

#### User Service
```bash
cd user-service
npm install
```

#### Course Service
```bash
cd course-service
npm install
```

#### Enrollment Service
```bash
cd enrollment-service
npm install
```

#### Progress Service
```bash
cd progress-service
npm install
```

#### Certificate Service
```bash
cd certificate-service
npm install
```

### 3. Configure Environment Variables

Each service has a `.env` file with default values. Update them as needed:

**Example User Service `.env`:**
```
PORT=5000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/online_course_platform_users
JWT_SECRET=your_jwt_secret_key_here
JWT_EXPIRE=7d
```

---

##  Running the Project

### Option 1: Local Development (Without Docker)

#### Start MongoDB
```bash
mongod
```

#### Terminal 1 - Start API Gateway
```bash
cd api-gateway
npm run dev
```

#### Terminal 2 - Start User Service
```bash
cd user-service
npm run dev
```

#### Terminal 3 - Start Course Service
```bash
cd course-service
npm run dev
```

#### Terminal 4 - Start Enrollment Service
```bash
cd enrollment-service
npm run dev
```

#### Terminal 5 - Start Progress Service
```bash
cd progress-service
npm run dev
```

#### Terminal 6 - Start Certificate Service
```bash
cd certificate-service
npm run dev
```

#### Terminal 7 - Start Frontend
```bash
cd frontend
npm run dev
```

### Option 2: Using Docker Compose

```bash
# Build and start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

---

## 🔌 API Endpoints

### User Service (via API Gateway)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/users/register` | Register a new user |
| POST | `/users/login` | User login |
| GET | `/users/:userId` | Get user profile |
| PUT | `/users/:userId` | Update user profile |
| DELETE | `/users/:userId` | Delete user |

### Course Service

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/courses` | Get all courses |
| GET | `/courses/:courseId` | Get course details |
| POST | `/courses` | Create new course |
| PUT | `/courses/:courseId` | Update course |
| DELETE | `/courses/:courseId` | Delete course |

### Enrollment Service

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/enrollments/enroll` | Enroll in a course |
| GET | `/enrollments/my-enrollments` | Get user enrollments |
| GET | `/enrollments/:enrollmentId` | Get enrollment details |
| PUT | `/enrollments/:enrollmentId/status` | Update enrollment status |
| DELETE | `/enrollments/:enrollmentId/cancel` | Cancel enrollment |

### Progress Service

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/progress/track` | Track lesson progress |
| GET | `/progress/my-progress` | Get user progress |
| GET | `/progress/:progressId` | Get progress details |
| PUT | `/progress/:progressId/score` | Update score |
| GET | `/progress/course/:courseId/stats` | Get course statistics |

### Certificate Service

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/certificates/generate` | Generate certificate |
| GET | `/certificates/my-certificates` | Get user certificates |
| GET | `/certificates/:certificateId` | Get certificate details |
| GET | `/certificates/verify/:certificateNumber` | Verify certificate |
| PUT | `/certificates/:certificateId/revoke` | Revoke certificate |

---

##  Project Structure

```
online-course-platform/
│
├── frontend/
│   ├── package.json
│   ├── index.html
│   ├── vite.config.js
│   ├── src/
│   │   ├── assets/images/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── App.css
│
├── api-gateway/
│   ├── package.json
│   ├── server.js
│   ├── .env
│   ├── routes/gatewayRoutes.js
│   └── config/gatewayConfig.js
│
├── user-service/
│   ├── package.json
│   ├── server.js
│   ├── .env
│   ├── routes/userRoutes.js
│   ├── controller/userController.js
│   ├── models/userModel.js
│   └── config/db.js
│
├── course-service/
│   ├── package.json
│   ├── server.js
│   ├── .env
│   ├── routes/courseRoutes.js
│   ├── controller/courseController.js
│   ├── models/courseModel.js
│   └── config/db.js
│
├── enrollment-service/
│   ├── package.json
│   ├── server.js
│   ├── .env
│   ├── routes/enrollmentRoutes.js
│   ├── controller/enrollmentController.js
│   ├── models/enrollmentModel.js
│   └── config/db.js
│
├── progress-service/
│   ├── package.json
│   ├── server.js
│   ├── .env
│   ├── routes/progressRoutes.js
│   ├── controller/progressController.js
│   ├── models/progressModel.js
│   └── config/db.js
│
├── certificate-service/
│   ├── package.json
│   ├── server.js
│   ├── .env
│   ├── routes/certificateRoutes.js
│   ├── controller/certificateController.js
│   ├── models/certificateModel.js
│   └── config/db.js
│
├── docker-compose.yml
└── README.md
```

---

##  Key Features

### User Management
- Register new users (students/instructors)
- Secure login with JWT tokens
- User profile management

### Course Management
- Create and manage courses
- Organize courses into modules
- Add lessons with video links
- Set course pricing and levels

### Student Learning
- Browse available courses
- Enroll in courses of interest
- Access course materials and lessons
- Track learning progress

### Progress Tracking
- Real-time lesson completion tracking
- Progress percentage calculation
- Performance scoring system
- Course statistics and analytics

### Certification
- Automatic certificate generation upon completion
- Unique certificate numbers for verification
- Grade assignment based on scores
- Certificate revocation capability

---

##  Security Features

- JWT-based authentication
- Password hashing with bcryptjs
- Environment variable configuration
- CORS protection
- Service isolation

---

##  Development Tips

1. **Keep services independent** - Each service should work independently
2. **Use proper error handling** - Implement error middleware in each service
3. **Follow RESTful standards** - Design APIs according to REST principles
4. **Version your APIs** - Consider adding API versioning (e.g., `/v1/users`)
5. **Add logging** - Implement comprehensive logging for debugging
6. **Test thoroughly** - Write unit and integration tests for each service
7. **Use MongoDB indexes** - Create indexes on frequently queried fields

---

##  Technologies Used

- **Frontend:** React 18, Vite, React Router, Axios
- **Backend:** Node.js, Express.js
- **Database:** MongoDB
- **Authentication:** JWT (JSON Web Tokens)
- **Security:** bcryptjs for password hashing
- **Containerization:** Docker, Docker Compose

---

##  Contributing

1. Create a new branch for each feature
2. Follow the project structure and conventions
3. Test your changes thoroughly
4. Ensure each service runs independently
5. Update documentation as needed

---


##  Troubleshooting

### MongoDB Connection Failed
- Ensure MongoDB is running: `mongod`
- Check the MongoDB connection string in `.env`
- Verify MongoDB credentials if using authentication

### Port Already in Use
- Change the port in the `.env` file
- Or kill the process using the port: `lsof -i :PORT` then `kill -9 PID`

### Service Not Responding
- Check if the service is running and healthy
- Verify the port and URL configuration
- Check the service logs for errors

### CORS Errors
- Ensure API Gateway has CORS enabled
- Check frontend requests are pointing to the correct API Gateway URL

---

## 📄 License

This project is created for educational purposes.

---

##  Happy Learning!

Start building your online course platform and expand your skills with microservices architecture!
