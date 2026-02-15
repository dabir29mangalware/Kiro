# CitySathi: Design Document
## The "Instagram" for Civic Governance

**Copyright © 2026 CitySathi Project. All rights reserved.**  
**License:** MIT License - See [LICENSE](LICENSE) file for details.

---

## ⚠️ Disclaimer

This document contains conceptual designs for the CitySathi platform. The ideas, architecture, and specifications described herein are the intellectual property of the CitySathi Project. While this document is publicly shared for collaboration and feedback, any implementation requires proper authorization and compliance with applicable laws.

---

## 1. Vision Statement

CitySathi transforms civic engagement from bureaucratic complaint filing into a social, visual, and community-driven experience. By combining the familiar interface of Instagram with the structured backend of CPGRAMS, we create a platform where citizens naturally participate in urban governance.

---

## 2. Core Design Philosophy

### 2.1 Visual-First Approach
- Every civic issue is a "post" with photo/video evidence
- Issues are discoverable through a location-based feed
- Status updates appear as "stories" when resolved

### 2.2 Community-Powered Prioritization
- Replace duplicate complaints with upvotes ("Support" button)
- Algorithmic priority: 1 Complaint + 50 Upvotes = High Priority
- Reduce system noise while amplifying citizen voice

### 2.3 Dual Identity Architecture
- **Frontend**: Social media experience (Instagram-like)
- **Backend**: Formal governance system (CPGRAMS-compliant)

---

## 3. User Experience Design

### 3.1 The Feed (Home Screen)

**Layout:**
```
┌─────────────────────────────┐
│  📍 Ward 4, Main Road       │
│  ─────────────────────────  │
│  [Photo: Pothole]           │
│                             │
│  ⚠️ Risk of Accident        │
│  Posted 2 hours ago         │
│                             │
│  👍 Support (47) | 📍 Track │
│  Status: ⏳ Pending         │
└─────────────────────────────┘
```

**Key Features:**
- Location-based feed showing issues in user's area
- Visual hierarchy: Image → Description → Action Bar
- Real-time status badges (Pending/In Progress/Resolved)

### 3.2 The Interaction Model

**Instead of "Like/Comment":**
- **Support (👍)**: Upvote the issue (increases priority)
- **Track (📍)**: Follow updates on this issue
- **Verify (✅)**: Confirm issue still exists (for ward officers)

**Posting Flow:**
1. Tap "+" button (like Instagram)
2. Capture photo/video
3. Auto-detect location via GPS
4. Add brief description
5. Select category (Roads/Water/Garbage/etc.)
6. Post → Automatically becomes CPGRAMS ticket

### 3.3 The Status Stories

**When Issues Get Resolved:**
- Ward Officer uploads "After" photo
- Appears as a circular story bubble at top of feed
- Shows before/after comparison
- Community can verify resolution

---

## 4. Technical Architecture

### 4.1 Frontend (Mobile App)

**Technology Stack:**
- React Native / Flutter (cross-platform)
- Map Integration: Google Maps API
- Camera: Native device camera with compression
- Offline Support: Queue posts when no connectivity

**UI Components:**
- Feed Component (scrollable list)
- Post Card (image, metadata, action bar)
- Story Ring (resolved issues)
- Map View (alternative to feed)

### 4.2 Backend (Spring Boot)

**Core Services:**
```
CitySathiApp
├── User Service (Authentication, Profiles)
├── Post Service (Issue Creation, Upvoting)
├── Location Service (Geo-tagging, Ward Mapping)
├── Priority Engine (Upvote-based ranking)
├── CPGRAMS Adapter (Formal ticket generation)
├── Notification Service (Push alerts)
└── Analytics Service (Dashboard for officials)
```

**Data Flow:**
1. User posts issue → Stored as "Social Post"
2. Background job converts to CPGRAMS ticket
3. Upvotes update priority score in real-time
4. Auto-escalation triggers if thresholds met
5. Resolution updates sync back to feed

### 4.3 Integration Layer

**CPGRAMS Compliance:**
- Every post generates a unique ticket ID
- Maps social categories to official complaint types
- Maintains audit trail for legal compliance
- Supports RTI (Right to Information) queries

---

## 5. Visual Design System

### 5.1 Color Palette

**Status Colors:**
- 🔴 Red: Urgent/Pending (0-48 hours)
- 🟡 Yellow: In Progress
- 🟢 Green: Resolved
- ⚫ Gray: Verified Closed

**Brand Colors:**
- Primary: #2563EB (Trust Blue)
- Secondary: #10B981 (Action Green)
- Accent: #F59E0B (Alert Orange)

### 5.2 Typography
- Headers: Bold, Sans-serif (Poppins/Inter)
- Body: Regular, Readable (16px minimum)
- Metadata: Light, Small (12px, gray)

### 5.3 Iconography
- Minimalist, line-based icons
- Consistent with Material Design / iOS guidelines
- Custom icons for civic categories

---

## 6. Key Screens & Wireframes

### 6.1 Feed Screen
```
┌───────────────────────────────┐
│ ☰  CitySathi      🔔  👤      │
├───────────────────────────────┤
│ 📍 Your Area: Ward 4          │
├───────────────────────────────┤
│ ○ ○ ○ ○  [Resolved Stories]  │
├───────────────────────────────┤
│ ┌─────────────────────────┐   │
│ │ [Image: Garbage Dump]   │   │
│ │                         │   │
│ │ Ward 4, MG Road         │   │
│ │ Health Hazard           │   │
│ │ 👍 52  📍 Track  ⏳     │   │
│ └─────────────────────────┘   │
│                               │
│ ┌─────────────────────────┐   │
│ │ [Image: Broken Street]  │   │
│ │                         │   │
│ │ Ward 4, Park Street     │   │
│ │ Safety Risk             │   │
│ │ 👍 23  📍 Track  ⏳     │   │
│ └─────────────────────────┘   │
├───────────────────────────────┤
│  🏠  🗺️  ➕  🔔  👤         │
└───────────────────────────────┘
```

### 6.2 Post Creation Screen
```
┌───────────────────────────────┐
│ ✕  New Issue          Post    │
├───────────────────────────────┤
│                               │
│   ┌─────────────────────┐     │
│   │                     │     │
│   │   [Camera Preview]  │     │
│   │                     │     │
│   └─────────────────────┘     │
│                               │
│ 📍 Ward 4, Main Road          │
│                               │
│ Category: [Roads ▼]           │
│                               │
│ Description:                  │
│ ┌─────────────────────────┐   │
│ │ Describe the issue...   │   │
│ └─────────────────────────┘   │
│                               │
│ Severity: ○ Low ● Med ○ High │
└───────────────────────────────┘
```

### 6.3 Issue Detail Screen
```
┌───────────────────────────────┐
│ ←  Issue Details              │
├───────────────────────────────┤
│ [Full Image]                  │
│                               │
│ Garbage Dump - Health Hazard  │
│ Ward 4, MG Road               │
│ Posted by @citizen123         │
│ 2 hours ago                   │
│                               │
│ ┌─────────────────────────┐   │
│ │ 👍 Support (52)         │   │
│ │ 📍 Track Updates        │   │
│ └─────────────────────────┘   │
│                               │
│ Status: ⏳ Pending            │
│ Ticket ID: #CS2024-1234       │
│                               │
│ Timeline:                     │
│ • Posted: 10:30 AM            │
│ • Acknowledged: 11:00 AM      │
│ • Assigned to: Ward Officer   │
│                               │
│ Similar Issues Nearby (3)     │
└───────────────────────────────┘
```

---

## 7. Smart Features

### 7.1 Auto-Escalation Logic
```
IF upvotes >= 100 AND status == "Pending" AND time > 48hrs
THEN escalate_to(Municipal_Commissioner)
```

### 7.2 Duplicate Detection
- AI-based image similarity matching
- Location proximity check (within 50m radius)
- Suggest existing post instead of creating new

### 7.3 Gamification (Optional)
- Citizen badges: "Active Reporter", "Community Hero"
- Ward Officer leaderboard: "Fastest Resolution"
- Monthly stats: "Issues Resolved in Your Area"

---

## 8. Accessibility & Inclusivity

- Multi-language support (Hindi, English, Regional)
- Voice-to-text for descriptions
- High contrast mode for visibility
- Works on low-end Android devices (Android 8+)
- Offline mode with sync when connected

---

## 9. Privacy & Security

- Anonymous posting option
- Location fuzzing (exact address hidden publicly)
- Moderation system for inappropriate content
- Data encryption in transit and at rest
- GDPR/Data Protection Act compliance

---

## 10. Success Metrics

**User Engagement:**
- Daily Active Users (DAU)
- Average posts per user per month
- Upvote participation rate

**Governance Impact:**
- Average resolution time (target: <72 hours)
- Reduction in duplicate complaints (target: 60%)
- Citizen satisfaction score (post-resolution survey)

**System Efficiency:**
- Auto-escalation accuracy
- CPGRAMS integration uptime (target: 99.5%)
- Mobile app crash rate (target: <1%)

---

## 11. Future Enhancements

### Phase 2:
- AR (Augmented Reality) for issue visualization
- Chatbot for status queries
- Integration with payment systems (for fines/fees)

### Phase 3:
- Predictive analytics (identify problem-prone areas)
- Citizen-to-Citizen marketplace (local services)
- Blockchain for transparency in fund allocation

---

## 12. Visual Mockup Descriptions

### Image 1: The Social Feed Concept
**Description:** A hand holding a smartphone displaying the CitySathi app. The screen shows an Instagram-like feed with civic issues instead of personal posts. A pothole photo has a red warning border, with stats showing "50 Residents Affected" instead of likes. Status badge shows "⚠️ Pending" in orange.

### Image 2: Community Power Illustration
**Description:** Split-screen cartoon illustration:
- **Left side:** Single person shouting at a large government building (ignored, small speech bubble fading away)
- **Right side:** Group of diverse people holding phones, connected by glowing lines to a central "HIGH PRIORITY" alert on a government computer screen. Visual shows collective power.

---

*Document Version: 1.0*  
*Last Updated: February 2026*
