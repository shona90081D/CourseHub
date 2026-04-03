# Online Course Platform - Architecture & Features Guide

## 🏗️ System Architecture

### Microservices Breakdown

```
┌────────────────────────────────────────────────────────────────┐
│                        FRONTEND LAYER                          │
│                   React 18 + Vite + Tailwind                   │
│                       (Port 3000)                              │
└───────────────────────────────┬────────────────────────────────┘
                                │
                                ▼
┌────────────────────────────────────────────────────────────────┐
│                    API GATEWAY (Express)                        │
│          Request Routing & CORS Management                     │
│                       (Port 4000)                              │
└──────────┬──────────┬─────────┬──────────┬─────────────┬───────┘
           │          │         │          │             │
     ┌─────▼──┐ ┌────▼──┐ ┌────▼──┐ ┌───▼───┐ ┌────▼─┐ ┌─▼────┐
     │ User   │ │Course │ │Enroll │ │Progress│ │Cert  │ │Others│
     │Service │ │Service│ │Service│ │Service│ │Service│ │      │
     │        │ │       │ │       │ │       │ │      │ │      │
     │ 5000   │ │ 5001  │ │ 5002  │ │ 5003  │ │ 5004 │ │...   │
     └─┬──────┘ └───┬───┘ └───┬───┘ └──┬────┘ └──┬───┘ └──┬───┘
       │            │         │        │         │        │
       └────────────┴─────────┴────────┴─────────┴────────┘
                              │
                              ▼
                    ┌──────────────────┐
                    │     MongoDB      │
                    │  (Port 27017)    │
                    │                  │
                    │ ├─ users         │
                    │ ├─ courses       │
                    │ ├─ enrollments   │
                    │ ├─ progress      │
                    │ └─ certificates  │
                    └──────────────────┘
```

---

## 📊 Data Model Relationships

```
User (1) ──────→ (N) Enrollment
          ├────→ (N) Progress
          └────→ (N) Certificate

Course (1) ────→ (N) Enrollment
          ├────→ (N) Progress
          └────→ (N) Certificate

Enrollment (1) ──→ (1) Certificate
            └────→ (1) Progress
```

---

## 🔄 Request Flow Example: Student Enrollment

```
1. User Registration/Login
   Frontend → API Gateway → User Service → MongoDB

2. Browse Courses
   Frontend → API Gateway → Course Service → MongoDB

3. Enroll in Course
   Frontend → API Gateway → Enrollment Service → MongoDB
   (Also triggers Course Service to update enrolledStudents)

4. Track Progress
   Frontend → API Gateway → Progress Service → MongoDB

5. Get Certificate
   Frontend → API Gateway → Certificate Service → MongoDB
   (Only if progress = 100%)
```

---

## 🗂️ Database Schema Overview

### Users Collection
```javascript
{
  _id: ObjectId,
  name: String,
  email: String (unique),
  password: String (hashed),
  role: String (student|instructor|admin),
  profilePicture: String,
  bio: String,
  isActive: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### Courses Collection
```javascript
{
  _id: ObjectId,
  title: String,
  description: String,
  instructor: String,
  instructorId: ObjectId,
  category: String,
  level: String (beginner|intermediate|advanced),
  duration: Number, // hours
  price: Number,
  thumbnail: String,
  modules: [{
    title: String,
    description: String,
    lessons: [{
      title: String,
      videoUrl: String,
      duration: Number
    }]
  }],
  enrolledStudents: Number,
  rating: Number (0-5),
  isPublished: Boolean,
  createdAt: Date,
  updatedAt: Date
}
```

### Enrollments Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  courseId: ObjectId,
  status: String (active|completed|cancelled),
  enrolledDate: Date,
  completionDate: Date,
  certificateId: ObjectId,
  paymentStatus: String (pending|completed|failed),
  courseDetails: {
    title: String,
    instructor: String,
    price: Number
  }
}
```

### Progress Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  courseId: ObjectId,
  courseName: String,
  totalLessons: Number,
  completedLessons: Number,
  completionPercentage: Number (0-100),
  status: String (not-started|in-progress|completed),
  lastAccessedDate: Date,
  lessons: [{
    lessonId: ObjectId,
    title: String,
    isCompleted: Boolean,
    completedDate: Date
  }],
  startDate: Date,
  completionDate: Date,
  score: Number (0-100),
  createdAt: Date
}
```

### Certificates Collection
```javascript
{
  _id: ObjectId,
  userId: ObjectId,
  courseId: ObjectId,
  courseName: String,
  studentName: String,
  instructorName: String,
  issueDate: Date,
  certificateNumber: String (unique),
  grade: String (A|B|C|D|F),
  score: Number (0-100),
  verificationUrl: String,
  issuedStatus: String (issued|revoked),
  skills: [String],
  createdAt: Date
}
```

---

## 🔐 Authentication Flow

```
1. User enters credentials → Frontend

2. Frontend sends → POST /users/login via API Gateway

3. User Service:
   - Finds user by email
   - Verifies password with bcrypt
   - Generates JWT token
   - Returns token & userId

4. Frontend stores token in localStorage

5. For protected routes:
   - Frontend includes Authorization header
   - API Gateway forwards with header
   - Service validates JWT token
   - Grants access if valid
```

### JWT Token Structure
```
Header: {
  "alg": "HS256",
  "typ": "JWT"
}

Payload: {
  "userId": "user-id",
  "email": "user@example.com",
  "role": "student",
  "iat": 1699000000,
  "exp": 1699604800
}

Signature: HMACSHA256(header + payload + secret)
```

---

## 🎯 Key Features & Implementation

### 1. User Authentication
**Services:** User Service
**Features:**
- Email/password registration
- Secure login with JWT
- Role-based access (student/instructor)
- Password hashing with bcryptjs

**Implementation:**
```
Register → Validate email → Hash password → Save user → Return JWT
Login → Find user → Verify password → Generate JWT → Return token
```

### 2. Course Management
**Services:** Course Service
**Features:**
- Create courses with modules and lessons
- Publish/unpublish courses
- Track enrollments
- Course categorization

**Implementation:**
```
Create → Validate data → Save to DB → Return course
List → Query DB → Filter by published → Return all courses
Update → Find course → Update fields → Save
```

### 3. Enrollment System
**Services:** Enrollment Service, Course Service
**Features:**
- Student enrollment in courses
- Multiple enrollment tracking
- Enrollment status management
- Payment status tracking

**Implementation:**
```
Enroll → Verify not already enrolled → Create enrollment record
         → Update course enrolledStudents count
Get Enrollments → Filter by userId → Return all enrollments
```

### 4. Progress Tracking
**Services:** Progress Service
**Features:**
- Lesson completion tracking
- Progress percentage calculation
- Real-time status updates
- Course analytics

**Implementation:**
```
Track → Find/Create progress record → Update lesson status
        → Calculate completion % → Update status (in-progress/completed)
Get Progress → Query by userId → Return progress records
Stats → Aggregate progress data → Calculate average completion
```

### 5. Certificate Generation
**Services:** Certificate Service
**Features:**
- Automatic generation on 100% completion
- Grade assignment based on score
- Certificate verification
- Unique certificate numbers

**Implementation:**
```
Generate → Check course completion → Assign grade → Create certificate
          → Generate unique number → Return certificate
Verify → Find by certificate number → Return certificate details
Revoke → Update status to "revoked" → Prevent re-verification
```

---

## 🌐 API Communication Patterns

### Direct Service-to-Service
```
Enrollment Service → Course Service (to update enrolled count)
Format: HTTP GET/POST to service URL directly
```

### Frontend → API Gateway → Service
```
Frontend makes request to API Gateway
API Gateway forwards to appropriate service
Service responds through API Gateway
Response returned to Frontend
```

### Error Propagation
```
Service Error → HTTP Status Code → API Gateway → Frontend
404 → Not Found
400 → Bad Request
500 → Internal Server Error
```

---

## 🚀 Performance Considerations

### Database Indexing
```javascript
// User Service
db.users.createIndex({ email: 1 }) // For fast login lookups

// Course Service
db.courses.createIndex({ category: 1 }) // For filtering by category
db.courses.createIndex({ instructorId: 1 }) // For instructor courses

// Enrollment Service
db.enrollments.createIndex({ userId: 1, courseId: 1 }) // Composite key

// Progress Service
db.progress.createIndex({ userId: 1 }) // For user progress queries

// Certificate Service
db.certificates.createIndex({ certificateNumber: 1 }) // For verification
db.certificates.createIndex({ userId: 1 }) // For user certificates
```

### Caching Strategies
```
1. Course List → Cache for 1 hour (changes infrequently)
2. User Progress → Cache for 5 minutes (frequent updates)
3. Certificates → Cache indefinitely (immutable)
```

### Query Optimization
```
1. Use pagination for large lists
2. Select only needed fields: `.select('-password')`
3. Use aggregation pipeline for complex stats
4. Denormalize frequently accessed data
```

---

## 🔄 Scalability Strategies

### Horizontal Scaling
```
Load Balancer
    ↓
├─ API Gateway Instance 1
├─ API Gateway Instance 2
└─ API Gateway Instance 3
    ↓
├─ User Service Instance 1
├─ User Service Instance 2
└─ User Service Instance 3
    (repeat for each service)
```

### Database Scaling
```
Master MongoDB Instance (writes)
    ↓
├─ Replica Set Member 1 (read)
├─ Replica Set Member 2 (read)
└─ Replica Set Member 3 (read)
```

### Caching Layer
```
Frontend
    ↓
Cache Layer (Redis)
    ↓
API Gateway
    ↓
Microservices
    ↓
MongoDB
```

---

## 📋 Service Independence Checklist

Each microservice should have:

- ✅ **Own Database** - Separate MongoDB database/collection
- ✅ **Own Server** - Independent Express server
- ✅ **Own Models** - Service-specific Mongoose schemas
- ✅ **Own Controllers** - Business logic implementation
- ✅ **Own Routes** - REST endpoints definition
- ✅ **Own .env** - Configuration variables
- ✅ **Own package.json** - Dependencies management
- ✅ **Error Handling** - Middleware for error management
- ✅ **Logging** - Service-level logging
- ✅ **Health Checks** - Status endpoint for monitoring

---

## 🧪 Testing Strategy

### Unit Testing
```javascript
// Test individual functions
- Controller functions
- Model validation
- Utility functions
```

### Integration Testing
```javascript
// Test service endpoints
- Test full API request flow
- Test database interactions
- Test error scenarios
```

### End-to-End Testing
```javascript
// Test complete workflow
- User registration → enrollment → progress → certificate
- Multi-service interactions
- Frontend → API Gateway → Services
```

### Load Testing
```javascript
// Performance testing
- Concurrent user requests
- Database query performance
- Service response times
```

---

## 📊 Monitoring & Analytics

### Key Metrics to Track

1. **API Response Times**
   - Average response time per endpoint
   - 95th percentile response time
   - Slow query identification

2. **Service Health**
   - Service uptime percentage
   - Error rate per service
   - Database connection health

3. **User Metrics**
   - Active users
   - Enrollment count
   - Course completion rate
   - Certificate generation rate

4. **System Performance**
   - Database load
   - Memory usage
   - CPU utilization
   - Network bandwidth

### Monitoring Tools
```
- Node.js APM: New Relic, Datadog, AppDynamics
- Logging: ELK Stack (Elasticsearch, Logstash, Kibana)
- Metrics: Prometheus + Grafana
- Uptime Monitoring: Pingdom, Uptime Robot
```

---

## 🔒 Security Best Practices

### Implemented
- ✅ JWT authentication
- ✅ Password hashing (bcryptjs)
- ✅ Environment variables
- ✅ CORS configuration

### Additional Recommendations
- 🔄 Rate limiting (express-rate-limit)
- 🔄 Input validation (joi, express-validator)
- 🔄 SQL injection prevention (already using MongoDB)
- 🔄 HTTPS/SSL certificates
- 🔄 API key management
- 🔄 Request logging
- 🔄 Dependency vulnerability scanning

---

## 📚 Extension Ideas

### Phase 2 Features
```
1. Advanced search and filtering
2. User roles and permissions
3. Real-time notifications (Socket.io)
4. Payment integration (Stripe, PayPal)
5. Email notifications
6. Course ratings and reviews
7. Student forums/discussions
8. Live class scheduling
9. Video streaming optimization
10. Data export/reports
```

### Phase 3 Features
```
1. Machine learning recommendations
2. Gamification (badges, leaderboards)
3. Peer-to-peer learning
4. Advanced analytics dashboard
5. Mobile app (React Native)
6. Multi-language support
7. Partner integrations
8. Advanced certificate features
9. AI-powered tutoring
10. Blockchain certificate verification
```

---

## 🎓 Learning Path for Developers

### Week 1: Foundation
- Understand microservices architecture
- Learn Express.js basics
- Setup MongoDB locally
- Get familiar with REST APIs

### Week 2: Core Services
- Implement User Service
- Implement Course Service
- Test with Postman

### Week 3: Integration
- Implement Enrollment Service
- Implement Progress Service
- Setup API Gateway

### Week 4: Certificate & Frontend
- Implement Certificate Service
- Build React frontend
- End-to-end testing

### Week 5: Deployment
- Docker containerization
- Docker Compose setup
- Deploy to cloud (AWS/Heroku)

### Week 6: Enhancement
- Add advanced features
- Performance optimization
- Security hardening

---

## 🚀 Production Deployment Checklist

- ✅ All services use environment variables
- ✅ Comprehensive error handling
- ✅ Database backups configured
- ✅ Monitoring and logging setup
- ✅ SSL/HTTPS certificates
- ✅ Rate limiting implemented
- ✅ Input validation on all endpoints
- ✅ CORS properly configured
- ✅ Database indexes created
- ✅ Load balancer configured
- ✅ CI/CD pipeline setup
- ✅ Disaster recovery plan

---

**This document provides a comprehensive overview of the Online Course Platform architecture. Use it as a reference during development and expansion!**
