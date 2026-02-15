# CitySathi: Requirements Document
## Functional & Non-Functional Requirements

**Copyright © 2026 CitySathi Project. All rights reserved.**  
**License:** MIT License - See [LICENSE](LICENSE) file for details.

---

## ⚠️ Disclaimer

This document contains technical requirements and specifications for the CitySathi platform. The requirements, user stories, and system designs described herein are the intellectual property of the CitySathi Project. While this document is publicly shared for collaboration and feedback, any implementation requires proper authorization and compliance with applicable laws.

---

## 1. Project Overview

**Project Name:** CitySathi  
**Tagline:** The "Instagram" for Civic Governance  
**Purpose:** Transform civic complaint filing into a social, visual, and community-driven experience while maintaining CPGRAMS compliance.

---

## 2. Stakeholders

### 2.1 Primary Users
- **Citizens:** Report issues, upvote existing complaints, track resolutions
- **Ward Officers:** Review, assign, and resolve issues
- **Municipal Commissioners:** Monitor high-priority escalations
- **System Administrators:** Manage platform, moderate content

### 2.2 External Systems
- **CPGRAMS:** Central Public Grievance Redress and Monitoring System
- **Municipal Databases:** Ward boundaries, officer assignments
- **Mapping Services:** Google Maps / OpenStreetMap

---

## 3. Functional Requirements

### 3.1 User Management

#### FR-1.1: User Registration
- Users must register using mobile number (OTP verification)
- Optional: Link Aadhaar for verified citizen status
- Profile includes: Name, Ward, Photo (optional)

#### FR-1.2: Authentication
- Support login via OTP, Google, or Facebook
- Session management with JWT tokens
- Role-based access control (Citizen, Officer, Admin)

#### FR-1.3: Profile Management
- Users can edit profile information
- View personal post history
- Track supported issues

---

### 3.2 Issue Posting (The "Instagram Post")

#### FR-2.1: Create New Issue
- **Required Fields:**
  - Photo/Video (max 10MB, compressed automatically)
  - Location (auto-detected via GPS, editable)
  - Category (dropdown: Roads, Water, Garbage, Electricity, etc.)
  - Description (max 500 characters)
  - Severity (Low/Medium/High)

#### FR-2.2: Duplicate Detection
- System checks for similar issues within 50m radius
- If found, prompt user to "Support" existing issue instead
- AI-based image similarity matching (optional Phase 2)

#### FR-2.3: Post Submission
- Generate unique Ticket ID (format: CS-YYYY-NNNN)
- Auto-tag with Ward number based on GPS
- Timestamp and user attribution
- Initial status: "Pending"

---

### 3.3 Feed & Discovery

#### FR-3.1: Location-Based Feed
- Default view: Issues within user's ward
- Sort options: Recent, Most Supported, Urgent
- Filter by: Category, Status, Distance

#### FR-3.2: Map View
- Alternative view showing issues as pins on map
- Color-coded by status (Red/Yellow/Green)
- Cluster nearby issues for clarity

#### FR-3.3: Search & Explore
- Search by location, category, or keyword
- Explore nearby wards (optional)

---

### 3.4 Community Interaction

#### FR-4.1: Support (Upvote)
- One support per user per issue
- Real-time counter update
- Notification to original poster on milestones (10, 50, 100 supports)

#### FR-4.2: Track Updates
- Users can "follow" an issue
- Receive push notifications on status changes
- View timeline of actions taken

#### FR-4.3: Verify Resolution
- Citizens can confirm issue is resolved
- Requires photo evidence (optional)
- Multiple verifications increase confidence score

---

### 3.5 Priority & Escalation Engine

#### FR-5.1: Priority Calculation
```
Priority Score = (Base_Severity × 10) + (Upvotes × 2) + (Time_Penalty)
- Base_Severity: Low=1, Medium=2, High=3
- Time_Penalty: +5 points per 24 hours pending
```

#### FR-5.2: Auto-Escalation Rules
- **Rule 1:** If upvotes ≥ 100 AND pending > 48 hours → Escalate to Commissioner
- **Rule 2:** If severity = High AND pending > 24 hours → Send alert to Ward Officer
- **Rule 3:** If upvotes ≥ 50 AND no acknowledgment in 12 hours → Escalate to Senior Officer

#### FR-5.3: Manual Escalation
- Ward Officers can manually escalate complex issues
- Requires justification note

---

### 3.6 Resolution & Status Updates

#### FR-6.1: Status Workflow
```
Pending → Acknowledged → In Progress → Resolved → Verified Closed
```

#### FR-6.2: Officer Actions
- Acknowledge issue (required within 24 hours)
- Assign to team/contractor
- Update status with notes
- Upload resolution photo (required for "Resolved")

#### FR-6.3: Resolution Stories
- Resolved issues appear as "story bubbles" at top of feed
- Show before/after photos
- Visible for 7 days, then archived

---

### 3.7 CPGRAMS Integration

#### FR-7.1: Ticket Generation
- Every post automatically creates CPGRAMS-compliant ticket
- Map CitySathi categories to CPGRAMS complaint types
- Sync ticket ID bidirectionally

#### FR-7.2: Data Synchronization
- Real-time status updates from CPGRAMS to CitySathi
- Daily batch sync for audit trail
- Handle conflicts (manual review queue)

#### FR-7.3: Audit & Compliance
- Maintain immutable log of all actions
- Support RTI (Right to Information) queries
- Generate compliance reports for government audits

---

### 3.8 Notifications

#### FR-8.1: Push Notifications
- Issue acknowledged by officer
- Status change on tracked issues
- Resolution of supported issues
- Milestone achievements (50 supports, etc.)

#### FR-8.2: In-App Notifications
- Notification center with read/unread status
- Deep links to relevant issues

---

### 3.9 Analytics & Reporting

#### FR-9.1: Citizen Dashboard
- Personal stats: Issues posted, supports given
- Ward statistics: Total issues, resolution rate
- Trending issues in area

#### FR-9.2: Officer Dashboard
- Assigned issues (sorted by priority)
- Performance metrics: Avg resolution time, pending count
- Ward-level heatmap

#### FR-9.3: Admin Dashboard
- System-wide statistics
- Ward comparison reports
- Escalation trends
- User engagement metrics

---

### 3.10 Content Moderation

#### FR-10.1: Automated Moderation
- Filter profanity in descriptions
- Block spam/duplicate posts from same user
- Flag inappropriate images (AI-based)

#### FR-10.2: Manual Moderation
- Admin review queue for flagged content
- User reporting mechanism ("Report Issue")
- Ability to hide/delete posts with reason

---

## 4. Non-Functional Requirements

### 4.1 Performance

#### NFR-1.1: Response Time
- Feed load: < 2 seconds
- Post submission: < 3 seconds
- Search results: < 1 second

#### NFR-1.2: Scalability
- Support 1 million concurrent users
- Handle 10,000 posts per hour during peak
- Database: Horizontal scaling with sharding

#### NFR-1.3: Availability
- Uptime: 99.5% (excluding planned maintenance)
- Graceful degradation if CPGRAMS API is down

---

### 4.2 Security

#### NFR-2.1: Data Protection
- Encrypt data in transit (TLS 1.3)
- Encrypt sensitive data at rest (AES-256)
- Anonymize location data (fuzzing to 100m radius)

#### NFR-2.2: Authentication & Authorization
- Multi-factor authentication for officers
- Rate limiting: 10 posts per user per day
- Session timeout: 30 minutes of inactivity

#### NFR-2.3: Privacy
- Users can post anonymously (hide name/photo)
- GDPR-compliant data deletion (within 30 days)
- No third-party data sharing without consent

---

### 4.3 Usability

#### NFR-3.1: Mobile-First Design
- Responsive UI for screens 4.5" to 6.5"
- Touch-friendly buttons (min 44×44 px)
- Intuitive navigation (max 3 taps to any feature)

#### NFR-3.2: Accessibility
- WCAG 2.1 Level AA compliance
- Screen reader support
- High contrast mode
- Font scaling (up to 200%)

#### NFR-3.3: Localization
- Support 10+ Indian languages (Hindi, Tamil, Bengali, etc.)
- Right-to-left (RTL) support for Urdu
- Date/time formatting per locale

---

### 4.4 Compatibility

#### NFR-4.1: Mobile Platforms
- Android 8.0+ (API level 26)
- iOS 13.0+
- Progressive Web App (PWA) for desktop

#### NFR-4.2: Browser Support (PWA)
- Chrome 90+
- Safari 14+
- Firefox 88+

#### NFR-4.3: Network Conditions
- Functional on 3G networks (min 1 Mbps)
- Offline mode: Queue posts, sync when online
- Image compression for low bandwidth

---

### 4.5 Maintainability

#### NFR-5.1: Code Quality
- Test coverage: > 80%
- Code documentation: JSDoc/JavaDoc
- Follow SOLID principles

#### NFR-5.2: Monitoring
- Real-time error tracking (Sentry/Rollbar)
- Performance monitoring (New Relic/Datadog)
- Logging: Centralized with ELK stack

#### NFR-5.3: Deployment
- CI/CD pipeline (GitHub Actions/Jenkins)
- Blue-green deployment for zero downtime
- Automated rollback on failure

---

### 4.6 Legal & Compliance

#### NFR-6.1: Data Residency
- All data stored in India (compliance with data localization laws)
- Backup data centers in different geographic zones

#### NFR-6.2: Terms of Service
- Clear user agreement on data usage
- Content policy (no hate speech, misinformation)
- Disclaimer on resolution timelines

---

## 5. System Constraints

### 5.1 Technical Constraints
- Must integrate with existing CPGRAMS API (REST-based)
- GPS accuracy: ±10 meters (device-dependent)
- Image storage: AWS S3 or equivalent (CDN required)

### 5.2 Business Constraints
- Free for citizens (no subscription model)
- Government-funded or CSR-sponsored
- Pilot launch in 1 city (3 months), then scale

### 5.3 Regulatory Constraints
- Compliance with IT Act 2000
- Adherence to municipal governance rules
- No political content or campaigning

---

## 6. Assumptions & Dependencies

### 6.1 Assumptions
- Users have smartphones with camera and GPS
- Municipal officers have basic digital literacy
- CPGRAMS API is stable and documented

### 6.2 Dependencies

#### 6.2.1 Government & Administrative
- Government approval for CPGRAMS integration
- Municipal cooperation for ward officer onboarding
- Access to ward boundary data and officer assignments

#### 6.2.2 Third-Party Services
- **Maps API:** Google Maps Platform / OpenStreetMap
- **SMS Gateway:** Twilio / AWS SNS for OTP verification
- **Cloud Hosting:** AWS / Azure / Google Cloud (India region)
- **Push Notifications:** Firebase Cloud Messaging (FCM) / Apple Push Notification Service (APNS)
- **CDN:** CloudFront / Cloudflare for image delivery
- **Email Service:** SendGrid / AWS SES for notifications

#### 6.2.3 Spring Boot Backend Dependencies

**Core Framework:**
```xml
<!-- Spring Boot Starter Parent -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.2.0</version>
</parent>

<!-- Spring Boot Web -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!-- Spring Boot Security -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<!-- Spring Boot Validation -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

**Database Dependencies:**
```xml
<!-- Spring Data JPA -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- MySQL Connector -->
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
    <version>8.2.0</version>
    <scope>runtime</scope>
</dependency>

<!-- PostgreSQL Driver (Alternative/Additional) -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>

<!-- HikariCP Connection Pool (included in spring-boot-starter-data-jpa) -->
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
</dependency>

<!-- Flyway for Database Migration -->
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
</dependency>

<!-- Flyway MySQL Support -->
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-mysql</artifactId>
</dependency>
```

**Spatial/GIS Support:**
```xml
<!-- Hibernate Spatial for Geographic Data -->
<dependency>
    <groupId>org.hibernate.orm</groupId>
    <artifactId>hibernate-spatial</artifactId>
    <version>6.4.0.Final</version>
</dependency>

<!-- JTS Topology Suite for Geometry -->
<dependency>
    <groupId>org.locationtech.jts</groupId>
    <artifactId>jts-core</artifactId>
    <version>1.19.0</version>
</dependency>
```

**Caching:**
```xml
<!-- Spring Boot Cache -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>

<!-- Redis for Distributed Caching -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!-- Lettuce Redis Client (included in spring-boot-starter-data-redis) -->
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
</dependency>
```

**Authentication & Security:**
```xml
<!-- JWT Token Support -->
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.3</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.3</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.3</version>
    <scope>runtime</scope>
</dependency>

<!-- OAuth2 Resource Server -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-oauth2-resource-server</artifactId>
</dependency>
```

**File Storage & Processing:**
```xml
<!-- AWS SDK for S3 -->
<dependency>
    <groupId>software.amazon.awssdk</groupId>
    <artifactId>s3</artifactId>
    <version>2.21.0</version>
</dependency>

<!-- Apache Commons FileUpload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.5</version>
</dependency>

<!-- Image Processing (Thumbnails) -->
<dependency>
    <groupId>net.coobird</groupId>
    <artifactId>thumbnailator</artifactId>
    <version>0.4.20</version>
</dependency>
```

**REST API & Integration:**
```xml
<!-- Spring Boot WebClient for HTTP Calls -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>

<!-- OpenAPI/Swagger Documentation -->
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.3.0</version>
</dependency>

<!-- Jackson for JSON Processing -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
</dependency>
```

**Messaging & Notifications:**
```xml
<!-- Spring Boot Mail -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>

<!-- Firebase Admin SDK for Push Notifications -->
<dependency>
    <groupId>com.google.firebase</groupId>
    <artifactId>firebase-admin</artifactId>
    <version>9.2.0</version>
</dependency>

<!-- Twilio SDK for SMS -->
<dependency>
    <groupId>com.twilio.sdk</groupId>
    <artifactId>twilio</artifactId>
    <version>9.14.1</version>
</dependency>
```

**Monitoring & Logging:**
```xml
<!-- Spring Boot Actuator -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<!-- Micrometer for Metrics -->
<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>

<!-- Logback (included in spring-boot-starter-logging) -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
</dependency>

<!-- SLF4J API -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
</dependency>
```

**Testing:**
```xml
<!-- Spring Boot Test -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>

<!-- JUnit 5 (included in spring-boot-starter-test) -->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <scope>test</scope>
</dependency>

<!-- Mockito -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <scope>test</scope>
</dependency>

<!-- H2 Database for Testing -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope>
</dependency>

<!-- TestContainers for Integration Testing -->
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>testcontainers</artifactId>
    <version>1.19.3</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.testcontainers</groupId>
    <artifactId>mysql</artifactId>
    <version>1.19.3</version>
    <scope>test</scope>
</dependency>
```

**Utilities:**
```xml
<!-- Lombok for Boilerplate Reduction -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>

<!-- MapStruct for DTO Mapping -->
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.5.5.Final</version>
</dependency>
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-processor</artifactId>
    <version>1.5.5.Final</version>
    <scope>provided</scope>
</dependency>

<!-- Apache Commons Lang -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-lang3</artifactId>
</dependency>

<!-- Google Guava -->
<dependency>
    <groupId>com.google.guava</groupId>
    <artifactId>guava</artifactId>
    <version>32.1.3-jre</version>
</dependency>
```

#### 6.2.4 Database Configuration

**MySQL Configuration (application.yml):**
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/citysathi?useSSL=false&serverTimezone=Asia/Kolkata
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
    driver-class-name: com.mysql.cj.jdbc.Driver
    hikari:
      maximum-pool-size: 20
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000
  
  jpa:
    hibernate:
      ddl-auto: validate
    show-sql: false
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
        format_sql: true
        use_sql_comments: true
        jdbc:
          batch_size: 20
        order_inserts: true
        order_updates: true
  
  flyway:
    enabled: true
    baseline-on-migrate: true
    locations: classpath:db/migration
```

**Database Schema Requirements:**
- MySQL 8.0+ or PostgreSQL 13+
- Support for spatial data types (POINT, POLYGON for geographic coordinates)
- UTF-8 character encoding for multi-language support
- Minimum storage: 100GB (scalable)
- Backup strategy: Daily automated backups with 30-day retention

---

## 7. User Stories

### 7.1 Citizen Stories

**US-1:** As a citizen, I want to see existing issues in my area so I can avoid filing duplicates.  
**Acceptance Criteria:**
- Feed shows issues within my ward by default
- Issues are sorted by recency
- I can filter by category

**US-2:** As a citizen, I want to support an existing issue so my voice is heard without creating noise.  
**Acceptance Criteria:**
- One-tap "Support" button on each post
- Counter updates in real-time
- I receive confirmation notification

**US-3:** As a citizen, I want to track the status of issues I care about so I know when they're resolved.  
**Acceptance Criteria:**
- "Track" button adds issue to my watchlist
- Push notification on status changes
- Timeline view shows all updates

### 7.2 Officer Stories

**US-4:** As a ward officer, I want to see high-priority issues first so I can address urgent problems.  
**Acceptance Criteria:**
- Dashboard sorts by priority score
- Color-coded urgency indicators
- One-click to acknowledge

**US-5:** As a ward officer, I want to upload resolution photos so citizens can verify the work.  
**Acceptance Criteria:**
- Camera integration in app
- Before/after photo comparison
- Automatic status change to "Resolved"

### 7.3 Admin Stories

**US-6:** As an admin, I want to monitor system-wide performance so I can identify bottlenecks.  
**Acceptance Criteria:**
- Real-time dashboard with key metrics
- Ward-wise comparison charts
- Export reports as PDF/Excel

---

## 8. Success Criteria

### 8.1 Launch Metrics (3 Months)
- 50,000+ registered users in pilot city
- 10,000+ issues posted
- 60% reduction in duplicate complaints
- Average resolution time < 72 hours

### 8.2 Long-Term Goals (1 Year)
- Expansion to 10 cities
- 1 million+ active users
- 80% citizen satisfaction score
- Integration with 5+ state CPGRAMS systems

---

## 9. Out of Scope (Phase 1)

- AI-based predictive analytics
- Augmented Reality (AR) features
- Payment gateway integration
- Citizen-to-Citizen marketplace
- Blockchain transparency layer

---

## 10. Risks & Mitigation

### 10.1 Technical Risks
**Risk:** CPGRAMS API downtime  
**Mitigation:** Queue tickets locally, sync when available

**Risk:** GPS inaccuracy in dense urban areas  
**Mitigation:** Allow manual location correction

### 10.2 Adoption Risks
**Risk:** Low citizen engagement  
**Mitigation:** Gamification, local influencer partnerships

**Risk:** Officer resistance to new system  
**Mitigation:** Training programs, phased rollout

### 10.3 Security Risks
**Risk:** Spam/fake posts  
**Mitigation:** Rate limiting, OTP verification, AI moderation

---

## 11. Acceptance Testing Checklist

- [ ] User can register and login successfully
- [ ] User can post issue with photo and location
- [ ] Duplicate detection suggests existing issues
- [ ] Upvote increments counter in real-time
- [ ] Auto-escalation triggers at defined thresholds
- [ ] Officer can update status and upload resolution photo
- [ ] CPGRAMS ticket is generated for every post
- [ ] Push notifications are received on status changes
- [ ] Feed loads within 2 seconds on 4G network
- [ ] App works offline and syncs when online
- [ ] Multi-language support functions correctly
- [ ] Admin dashboard displays accurate analytics

---

## 12. Glossary

- **Upvote:** Citizen action to support an existing issue (increases priority)
- **Ward:** Administrative subdivision of a city
- **CPGRAMS:** Central Public Grievance Redress and Monitoring System
- **Escalation:** Automatic or manual promotion of issue to higher authority
- **Story:** Resolved issue displayed as temporary update (Instagram-style)
- **Ticket ID:** Unique identifier for each issue (format: CS-YYYY-NNNN)
- **Priority Score:** Calculated metric determining issue urgency

---

*Document Version: 1.0*  
*Last Updated: February 2026*  
*Status: Draft for Idea Submission*
