# 🩺 On My Way Therapy Platform

[← Back to All Projects](../PROJECTS.md)

---

## 📋 Project Overview

**Client:** Cross Care (Australia)  
**Sub-Company:** On My Way Therapy  
**Duration:** September 2022 – May 2023  
**Project Type:** Healthcare SaaS Platform  
**Role:** Full-Stack Developer & Project Manager  
**Status:** Successfully Deployed & Featured on Shark Tank Australia

---

## 🎯 Project Background

Cross Care, a leading Australian healthcare provider, launched **On My Way Therapy** as a specialized platform for therapy services. The platform needed to:

- Connect therapists with clients requiring in-home therapy sessions
- Comply with Australia's NDIS (National Disability Insurance Scheme) regulations
- Provide scheduling, location tracking, and session management
- Support secure billing and documentation
- Facilitate communication between therapists, clients, and NDIS coordinators

**Critical Requirement:** Full NDIS compliance, including accessibility standards, data privacy regulations, and billing integration with NDIS funding schemes.

---

## 🏗️ System Architecture

### Platform Structure
```
┌──────────────────────────────────────────┐
│          Laravel Backend API             │
│    (Booking, Scheduling, Compliance)     │
└────────────────┬─────────────────────────┘
                 │
        ┌────────┴────────┐
        │                 │
  ┌─────▼──────┐    ┌────▼─────┐
  │   React    │    │  Mobile  │
  │ Web Portal │    │   Apps   │
  └────────────┘    └──────────┘
```

### Technology Stack
- **Backend:** Laravel (PHP) with RESTful API
- **Frontend:** React with responsive design
- **Database:** MySQL with NDIS-compliant encryption
- **Maps:** Google Maps API (location, routing, distance)
- **Storage:** Secure cloud storage (HIPAA-compliant equivalent)
- **Authentication:** Multi-factor authentication for security

---

## 🔧 My Responsibilities

As the sole full-stack developer and project manager, I handled:

1. **Project Management**
   - Direct communication with Australian stakeholders
   - Timeline planning and milestone delivery
   - Feature prioritization and scope management
   - Coordination with NDIS compliance advisors

2. **Full-Stack Development**
   - Complete backend API development
   - Frontend web portal
   - Database design and implementation
   - Third-party integrations

3. **Compliance & Security**
   - NDIS regulation research and implementation
   - Accessibility standards (WCAG 2.1)
   - Data privacy and security measures
   - Audit trail implementation

---

## 🎨 Core Features Developed

### 1. User Management System

**Multi-Role Platform:**

**Therapists:**
- Professional profile with qualifications
- NDIS provider registration number
- Service areas and specializations
- Availability calendar
- Session notes and documentation
- Billing and invoicing

**Clients:**
- Client profile with NDIS plan details
- Medical history and requirements
- Guardian/family member access
- Session booking
- Document storage
- Payment tracking

**NDIS Coordinators:**
- Client oversight and monitoring
- Funding approval
- Progress reports
- Service verification

**Administrators:**
- Platform management
- User verification
- Analytics and reporting
- Compliance monitoring

### 2. Booking & Scheduling System

**Session Booking:**
- Search therapists by specialization, location, and availability
- Real-time availability checking
- Recurring appointment scheduling
- Session type selection (in-home, clinic, telehealth)
- Special requirements notation
- Automated confirmation emails

**Therapist Scheduling:**
- Interactive calendar interface
- Availability block management
- Travel time calculation
- Buffer time between sessions
- Session conflict prevention
- Route optimization for in-home visits

**Technical Implementation:**
```php
// Route optimization for therapist's daily schedule
function optimizeTherapistRoute($therapist, $date) {
    $sessions = $therapist->sessions()
        ->whereDate('scheduled_at', $date)
        ->with('client')
        ->get();
    
    // Get coordinates for all session locations
    $waypoints = $sessions->map(function($session) {
        return [
            'lat' => $session->client->latitude,
            'lng' => $session->client->longitude
        ];
    });
    
    // Use Google Maps Directions API for optimal route
    $optimizedRoute = $this->googleMaps
        ->optimizeRoute($waypoints);
    
    return $optimizedRoute;
}
```

### 3. Location & Navigation Features

**Google Maps Integration:**

**For Therapists:**
- Turn-by-turn navigation to client locations
- Real-time traffic updates
- Distance and travel time calculation
- Route history tracking
- "On My Way" status with ETA sharing

**For Clients:**
- Therapist real-time location tracking
- Estimated arrival time
- SMS notifications when therapist is nearby
- Visual map showing therapist approaching

**Implementation Details:**
```javascript
// Real-time therapist location tracking
const trackTherapistLocation = (sessionId) => {
  const updateInterval = setInterval(async () => {
    const location = await getCurrentPosition();
    
    // Update backend with current location
    await api.post('/sessions/update-location', {
      session_id: sessionId,
      latitude: location.coords.latitude,
      longitude: location.coords.longitude
    });
    
    // Calculate ETA
    const eta = await calculateETA(
      location,
      session.client_location
    );
    
    // Notify client when within 10 minutes
    if (eta <= 10 && !notificationSent) {
      sendClientNotification('Therapist arriving soon!');
    }
  }, 30000); // Update every 30 seconds
};
```

### 4. NDIS Compliance Features

**NDIS Plan Management:**
- NDIS plan number verification
- Funding category allocation
- Budget tracking per service category
- Automatic claim generation
- NDIS portal integration preparation

**Service Documentation:**
- Session attendance verification
- Progress notes templates
- Goal tracking aligned with NDIS outcomes
- Evidence collection for claims
- Digital signature capture

**Billing & Claims:**
- NDIS price guide compliance
- Automated invoice generation
- Claim submission tracking
- Payment reconciliation
- GST calculations

**Accessibility Compliance (WCAG 2.1):**
- Screen reader compatibility
- Keyboard navigation support
- High contrast mode
- Adjustable font sizes
- Alternative text for images
- Form validation with clear error messages

### 5. Communication System

**In-App Messaging:**
- Secure encrypted messaging
- Therapist-client communication
- File sharing (documents, images)
- Message history with search
- Read receipts

**Notification System:**
- Email notifications
- SMS alerts
- Push notifications (future mobile app)
- Reminder system (24hr, 2hr before session)
- Cancellation notifications

### 6. Session Management

**Session Workflow:**
```
Booking → Confirmation → Therapist En Route → 
Session In Progress → Session Completed → 
Notes & Documentation → Billing
```

**Session Features:**
- Check-in/check-out system
- Session duration tracking
- Service type recording
- Notes and observations
- Photo documentation
- Client signature collection
- Post-session survey

**Session Notes:**
```php
// Template-based session notes for NDIS compliance
class SessionNote extends Model
{
    protected $fillable = [
        'session_id',
        'activities_performed',
        'goals_addressed',
        'client_progress',
        'recommendations',
        'next_session_plan',
        'ndis_outcomes_met',
        'therapist_signature',
        'client_signature',
        'completed_at'
    ];
    
    // Automatic NDIS outcome mapping
    public function getNdisOutcomes() {
        return $this->goals_addressed()
            ->map(function($goal) {
                return $goal->ndis_outcome_domain;
            })
            ->unique();
    }
}
```

### 7. Analytics & Reporting

**Therapist Analytics:**
- Session statistics
- Revenue tracking
- Travel distance/time
- Client satisfaction scores
- Utilization rates

**Client Analytics:**
- Session attendance
- Progress tracking
- Goal achievement
- Funding utilization
- Service history

**Administrative Reports:**
- NDIS compliance reports
- Financial summaries
- User growth metrics
- Service demand analysis
- Geographic coverage maps

---

## 💡 Key Technical Challenges & Solutions

### Challenge 1: NDIS Compliance
**Problem:** Complex and strict NDIS regulations for healthcare data  
**Solution:**
- Extensive research of NDIS Provider Toolkit
- Consultation with NDIS compliance experts
- Implementation of all required data fields
- Audit trail for all transactions
- Encrypted storage for sensitive information

### Challenge 2: Accessibility Standards
**Problem:** Meeting WCAG 2.1 AA standards for diverse user abilities  
**Solution:**
- Semantic HTML structure
- ARIA labels for screen readers
- Keyboard navigation throughout
- Color contrast testing
- User testing with people with disabilities

### Challenge 3: Real-Time Location Tracking
**Problem:** Accurate ETA calculation with varying traffic conditions  
**Solution:**
- Google Maps Directions API with traffic data
- Background location updates
- Server-side ETA calculation
- Client-side progressive updates
- Battery optimization for mobile devices

### Challenge 4: International Remote Collaboration
**Problem:** Working with Australian client from Egypt (time zone, communication)  
**Solution:**
- Scheduled overlap hours for real-time communication
- Detailed documentation and video demos
- Asynchronous communication tools (Slack)
- Weekly video conferences
- Cultural sensitivity and professional communication

### Challenge 5: Data Privacy & Security
**Problem:** Healthcare data requires maximum security  
**Solution:**
- End-to-end encryption for messages
- Role-based access control
- Two-factor authentication
- Regular security audits
- HTTPS everywhere
- Data backup and disaster recovery

### Challenge 6: Distance & Time Calculations
**Problem:** Accurate travel time essential for scheduling  
**Solution:**
```php
// Accurate distance and time calculation
function calculateTravelDetails($origin, $destination) {
    $response = Http::get('https://maps.googleapis.com/maps/api/distancematrix/json', [
        'origins' => "{$origin->lat},{$origin->lng}",
        'destinations' => "{$destination->lat},{$destination->lng}",
        'mode' => 'driving',
        'departure_time' => 'now', // Real-time traffic
        'key' => config('services.google.maps_key')
    ]);
    
    $data = $response->json();
    
    return [
        'distance' => $data['rows'][0]['elements'][0]['distance']['value'], // meters
        'duration' => $data['rows'][0]['elements'][0]['duration_in_traffic']['value'], // seconds
        'duration_text' => $data['rows'][0]['elements'][0]['duration_in_traffic']['text']
    ];
}
```

---

## 📊 Project Outcomes

### Technical Success
✅ Full NDIS compliance achieved  
✅ WCAG 2.1 AA accessibility standards met  
✅ Real-time location tracking implemented  
✅ Secure healthcare data handling  
✅ Cross-timezone project delivery  

### Business Impact
✅ **Featured on Shark Tank Australia**  
✅ **Successfully secured investor funding**  
✅ Platform operational and serving Australian clients  
✅ Enabled Cross Care's expansion into therapy services  
✅ Positive user feedback from both therapists and clients  

### Personal Achievement
✅ First international client project  
✅ Experience with healthcare compliance  
✅ Project management of complete platform  
✅ Cross-cultural professional collaboration  
✅ Recognition on national TV (Shark Tank)  

---

## 🎓 Skills & Technologies Gained

### Healthcare Technology
- **NDIS Regulations:** Understanding Australian disability services framework
- **Healthcare Compliance:** HIPAA-equivalent data protection
- **Accessibility:** WCAG 2.1 implementation
- **Medical Documentation:** Clinical note templates and standards

### Location Services
- **Google Maps API:**
  - Distance Matrix API
  - Directions API
  - Geocoding
  - Real-time traffic data
  - Route optimization

### International Collaboration
- **Remote Work:** Managing project across 7-hour time difference
- **Cross-Cultural Communication:** Professional communication with Australian stakeholders
- **Documentation:** Clear technical and user documentation
- **Agile Practices:** Remote sprint planning and execution

### Business Understanding
- **Healthcare Business Models:** Therapy service operations
- **Investor Presentations:** Platform showcased to investors
- **Funding Models:** NDIS funding and billing
- **Market Analysis:** Australian healthcare market

---

## 🎬 Shark Tank Australia Feature

The platform was featured on Shark Tank Australia, where Cross Care pitched the business to potential investors. Key highlights:

**Pitch Focus:**
- Addressing accessibility gaps in therapy services
- NDIS-compliant platform reducing administrative burden
- Scalable SaaS model for nationwide expansion
- Technology enabling better therapist-client matching

**Outcome:**
- Successfully secured investment
- National TV exposure
- Validation of platform's business model
- Increased user sign-ups post-episode

**Impact on Project:**
- Demonstrated real-world viability
- Validated technical decisions
- Increased stakeholder confidence
- Opportunities for platform enhancement

---

## 🔮 Lessons Learned

1. **Compliance is Critical:** Healthcare platforms require extensive regulatory knowledge
2. **Accessibility Matters:** Designing for all abilities from day one is essential
3. **International Communication:** Clear documentation overcomes timezone barriers
4. **User-Centric Design:** Healthcare platforms must prioritize ease of use
5. **Location Accuracy:** Reliable location services are fundamental for in-home services

---

## 📞 Project Inquiries

Interested in this healthcare compliance project?

📧 **Email:** kareem.mohamed.swd@gmail.com  
🔗 **LinkedIn:** [kareem-mohamed-b3b676261](https://linkedin.com/in/kareem-mohamed-b3b676261)

---

[← Back to All Projects](../PROJECTS.md) | [View Next Project →](./uma-elearning.md)
