# 📂 Online Course Platform - Complete Directory Tree

```
Online Course Platform/
│
├── 📄 GETTING_STARTED.md           ⭐ START HERE - Quick setup guide
├── 📄 README.md                    📚 Full documentation & features
├── 📄 ARCHITECTURE.md              🏗️  System design & patterns
├── 📄 PROJECT_SUMMARY.md           📋 Project overview
├── 📄 SAMPLE_DATA.md               🧪 Test data & API examples
├── .gitignore                      🔒 Git ignore rules
│
├── 🗂️  FRONTEND APPLICATION (React 18 + Vite)
│   └── frontend/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 vite.config.js
│       ├── 📄 index.html
│       │
│       └── src/
│           ├── 📄 App.jsx                 Main React App
│           ├── 📄 main.jsx                Entry point
│           ├── 📄 App.css                 Global styles
│           │
│           ├── assets/
│           │   └── images/                Image assets folder
│           │
│           ├── components/
│           │   ├── 📄 Navbar.jsx          Navigation component
│           │   ├── 📄 Footer.jsx          Footer component
│           │   └── 📄 CourseCard.jsx      Course card display
│           │
│           ├── pages/
│           │   ├── 📄 Home.jsx            Browse courses
│           │   ├── 📄 Login.jsx           User login page
│           │   ├── 📄 Register.jsx        User registration
│           │   ├── 📄 Dashboard.jsx       User dashboard
│           │   └── 📄 CourseDetails.jsx   Course details
│           │
│           └── services/
│               └── 📄 api.js              API client (Axios)
│
├── 🗂️  API GATEWAY (Request Router - Port 4000)
│   └── api-gateway/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 server.js                  Main server
│       ├── 📄 .env                       Configuration
│       │
│       ├── routes/
│       │   └── 📄 gatewayRoutes.js       All gateway routes
│       │
│       └── config/
│           └── 📄 gatewayConfig.js       Configuration setup
│
├── 🗂️  USER SERVICE (Authentication - Port 5000)
│   └── user-service/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 server.js                  Main server
│       ├── 📄 .env                       DB & JWT config
│       │
│       ├── routes/
│       │   └── 📄 userRoutes.js          User endpoints
│       │
│       ├── controller/
│       │   └── 📄 userController.js      Business logic
│       │                                 - register()
│       │                                 - login()
│       │                                 - getUserById()
│       │                                 - updateProfile()
│       │                                 - deleteUser()
│       │
│       ├── models/
│       │   └── 📄 userModel.js           User schema
│       │                                 Fields:
│       │                                 - name, email, password
│       │                                 - role (student/instructor)
│       │                                 - profilePicture, bio
│       │
│       └── config/
│           └── 📄 db.js                  MongoDB connection
│
├── 🗂️  COURSE SERVICE (Course Management - Port 5001)
│   └── course-service/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 server.js                  Main server
│       ├── 📄 .env                       DB config
│       │
│       ├── routes/
│       │   └── 📄 courseRoutes.js        Course endpoints
│       │
│       ├── controller/
│       │   └── 📄 courseController.js    Business logic
│       │                                 - getAllCourses()
│       │                                 - getCourseById()
│       │                                 - createCourse()
│       │                                 - updateCourse()
│       │                                 - deleteCourse()
│       │
│       ├── models/
│       │   └── 📄 courseModel.js         Course schema
│       │                                 Fields:
│       │                                 - title, description
│       │                                 - instructor, category
│       │                                 - duration, price
│       │                                 - modules with lessons
│       │
│       └── config/
│           └── 📄 db.js                  MongoDB connection
│
├── 🗂️  ENROLLMENT SERVICE (Student Enrollment - Port 5002)
│   └── enrollment-service/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 server.js                  Main server
│       ├── 📄 .env                       DB & JWT config
│       │
│       ├── routes/
│       │   └── 📄 enrollmentRoutes.js    Enrollment endpoints
│       │
│       ├── controller/
│       │   └── 📄 enrollmentController.js Business logic
│       │                                 - enrollStudent()
│       │                                 - getUserEnrollments()
│       │                                 - getEnrollmentById()
│       │                                 - updateEnrollmentStatus()
│       │                                 - cancelEnrollment()
│       │
│       ├── models/
│       │   └── 📄 enrollmentModel.js     Enrollment schema
│       │                                 Fields:
│       │                                 - userId, courseId
│       │                                 - status (active/completed)
│       │                                 - enrolledDate, paymentStatus
│       │
│       └── config/
│           └── 📄 db.js                  MongoDB connection
│
├── 🗂️  PROGRESS SERVICE (Progress Tracking - Port 5003)
│   └── progress-service/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 server.js                  Main server
│       ├── 📄 .env                       DB & JWT config
│       │
│       ├── routes/
│       │   └── 📄 progressRoutes.js      Progress endpoints
│       │
│       ├── controller/
│       │   └── 📄 progressController.js  Business logic
│       │                                 - trackProgress()
│       │                                 - getUserProgress()
│       │                                 - updateScore()
│       │                                 - getCourseProgressStats()
│       │
│       ├── models/
│       │   └── 📄 progressModel.js       Progress schema
│       │                                 Fields:
│       │                                 - userId, courseId
│       │                                 - completedLessons
│       │                                 - completionPercentage
│       │                                 - lessons array
│       │
│       └── config/
│           └── 📄 db.js                  MongoDB connection
│
├── 🗂️  CERTIFICATE SERVICE (Certificate Generation - Port 5004)
│   └── certificate-service/
│       ├── 📄 package.json
│       ├── 📄 Dockerfile
│       ├── 📄 server.js                  Main server
│       ├── 📄 .env                       DB & JWT config
│       │
│       ├── routes/
│       │   └── 📄 certificateRoutes.js   Certificate endpoints
│       │
│       ├── controller/
│       │   └── 📄 certificateController.js Business logic
│       │                                 - generateCertificate()
│       │                                 - getUserCertificates()
│       │                                 - verifyCertificate()
│       │                                 - revokeCertificate()
│       │
│       ├── models/
│       │   └── 📄 certificateModel.js    Certificate schema
│       │                                 Fields:
│       │                                 - userId, courseId
│       │                                 - grade, score
│       │                                 - certificateNumber
│       │                                 - skills array
│       │
│       └── config/
│           └── 📄 db.js                  MongoDB connection
│
└── 🐳 DOCKER CONFIGURATION
    └── 📄 docker-compose.yml            Container orchestration
                                         Starts:
                                         - MongoDB
                                         - API Gateway
                                         - All 6 services
                                         - Frontend (optional)
```

---

## 📊 Service Hierarchy

```
                          🌐 Frontend
                                ↓
                          🚪 API Gateway
                    (Port 4000 - Central Hub)
                    ↙     ↓      ↓      ↓      ↘
        ┌─────────┴──────┼──────┼──────┼──────┴────────┐
        │                │      │      │                │
        ▼                ▼      ▼      ▼                ▼
    👤 User Service  📚 Course  🎓 Enrollment  📊 Progress  📜 Certificate
    (Port 5000)     Service   Service        Service      Service
                    (5001)    (5002)         (5003)      (5004)
        │                │      │      │                │
        └─────────┬──────┼──────┼──────┼──────┬────────┘
                  │                          │
                  ▼                          ▼
            🗄️ MongoDB (Port 27017)
            ├─ users_db
            ├─ courses_db
            ├─ enrollments_db
            ├─ progress_db
            └─ certificates_db
```

---

## 📈 File Count Summary

| Component | Files |
|-----------|-------|
| Frontend | 15 files |
| API Gateway | 5 files |
| User Service | 7 files |
| Course Service | 7 files |
| Enrollment Service | 7 files |
| Progress Service | 7 files |
| Certificate Service | 7 files |
| Configuration Files | 6 files |
| Documentation | 6 files |
| **TOTAL** | **69+ files** |

---

## 🔑 Key Files to Know

### Essential Setup Files
```
1. GETTING_STARTED.md      ← Start here!
2. docker-compose.yml      ← Run everything with Docker
3. .env (in each service)  ← Configuration
```

### Important Code Files
```
1. api-gateway/routes/gatewayRoutes.js      ← All routing logic
2. user-service/controller/userController.js ← Auth logic
3. course-service/models/courseModel.js     ← Course structure
4. frontend/src/App.jsx                     ← React app
```

### Database Files
```
1. All */models/*Model.js files              ← Database schemas
2. All */config/db.js files                  ← DB connection
```

---

## 🎯 How to Navigate

### For Setup & Quick Start
→ Read: `GETTING_STARTED.md`

### For Full Documentation
→ Read: `README.md`

### For Architecture Understanding
→ Read: `ARCHITECTURE.md`

### For Testing & API Examples
→ Read: `SAMPLE_DATA.md`

### For Project Overview
→ Read: `PROJECT_SUMMARY.md`

### For This Directory Structure
→ You're reading: `DIRECTORY_TREE.md`

---

## 🚀 File Organization Principles

✅ **Each service is independent** - Can run separately
✅ **Clear separation of concerns** - Routes, Controllers, Models
✅ **Consistent structure** - Same pattern across services
✅ **Environment-based config** - .env files for configuration
✅ **Scalable design** - Easy to add new services
✅ **Docker-ready** - Dockerfiles for each service
✅ **Well-documented** - README files and inline comments

---

## 💡 Tips for Working with This Structure

1. **Start with Frontend folder** - Understand the UI first
2. **Test API Gateway routes** - Use Postman
3. **Explore one service at a time** - Master User Service first
4. **Check .env files** - Understand configurations
5. **Read models** - Understand data structure
6. **Follow controllers** - See business logic
7. **Review routes** - Understand endpoints

---

## 🔄 Typical Development Workflow

```
1. Start MongoDB
   → mongod

2. Start API Gateway
   → api-gateway/ → npm run dev

3. Start all Services
   → 6 terminal windows, each service in one

4. Start Frontend
   → frontend/ → npm run dev

5. Open Browser
   → http://localhost:3000

6. Test Features
   → Register, Login, Enroll, Track, Generate Certificate
```

---

## 📚 Learning Path

```
Week 1: Understanding Structure
├─ Read all documentation
├─ Understand microservices concept
└─ Analyze project structure

Week 2: Database & Models
├─ Study MongoDB schemas
├─ Understand relationships
└─ Review all model files

Week 3: Backend Services
├─ Understand User Service
├─ Learn Course Service
├─ Study Enrollment logic

Week 4: Frontend
├─ Explore React components
├─ Understand API integration
└─ Test all features

Week 5: Integration & Testing
├─ Test all endpoints
├─ Verify data flow
└─ Check error handling

Week 6: Deployment
├─ Learn Docker
├─ Setup docker-compose
└─ Deploy to cloud
```

---

**This project is ready for development!** 🎉

Start with: `GETTING_STARTED.md`
