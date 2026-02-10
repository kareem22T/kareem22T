# 🛴 Fentec Mobility – E-Scooter Platform

[← Back to All Projects](../PROJECTS.md)

---

## 📋 Project Overview

**Client:** Fentec Mobility (Algeria)  
**Duration:** 2022 – 2023  
**Project Type:** Mobility-as-a-Service Platform  
**Role:** Full-Stack Developer & IoT Integration Specialist  
**Status:** Deployed to Production & Live in Algeria

---

## 🎯 Project Vision

Fentec Mobility aimed to launch a comprehensive electric scooter sharing platform in Algeria, similar to international solutions like Lime, Bird, and Tier. The platform needed to support:

- Fleet management for e-scooter operations
- Consumer mobile applications (iOS & Android)
- Real-time scooter tracking and availability
- Payment processing and accounting
- IoT integration with physical scooters
- Geographic zone management and geofencing

---

## 🏗️ System Architecture

### Complete Platform Ecosystem

```
┌──────────────────────────────────────────────────────────┐
│                 Administrative Dashboard                  │
│              (Fleet & Business Management)                │
└────────────────────────┬─────────────────────────────────┘
                         │
          ┌──────────────┼──────────────┐
          │              │              │
    ┌─────▼─────┐  ┌────▼────┐  ┌─────▼──────┐
    │  Laravel  │  │  React  │  │   React    │
    │  Backend  │  │ Native  │  │   Native   │
    │    API    │  │   iOS   │  │   Android  │
    └─────┬─────┘  └─────────┘  └────────────┘
          │
          │
    ┌─────▼────────────────────┐
    │  IoT Scooter Hardware    │
    │  (Lock/Unlock, GPS, etc) │
    └──────────────────────────┘
```

---

## 🔧 Technical Stack

### Backend
- **Framework:** Laravel (PHP)
- **Database:** MySQL with spatial data types
- **Caching:** Redis for real-time data
- **Queue:** Laravel Queue for job processing
- **Storage:** Cloud storage for images and documents

### Mobile Applications
- **Framework:** React Native
- **Platforms:** iOS (App Store) & Android (Google Play Store)
- **State Management:** Redux
- **Maps:** Google Maps SDK
- **Payments:** Stripe SDK integration

### Admin Dashboard
- **Framework:** React with Tailwind CSS
- **Charts:** Chart.js for analytics
- **Maps:** Google Maps JavaScript API
- **Real-time Updates:** WebSockets

### IoT Integration
- **Communication:** HTTP/MQTT protocols
- **Hardware:** GPS modules, electronic locks
- **Queue System:** Job queues for command processing
- **Real-time Tracking:** GPS coordinate updates

---

## 🎨 Core Features Developed

### 1. Mobile Applications (iOS & Android)

#### User Features
**Account Management:**
- User registration and authentication
- Profile management with photo upload
- Payment method management
- Ride history
- Balance and wallet system

**Scooter Discovery:**
- Real-time map showing available scooters nearby
- Scooter details (battery level, model, pricing)
- Distance to nearest scooter
- Reservation system
- Favorite locations

**Ride Experience:**
- QR code scanning to unlock scooter
- Real-time ride tracking with GPS
- Distance and time tracking
- Speed monitoring
- Turn-by-turn navigation
- Ride pause functionality
- End ride and automatic lock

**Payment & Billing:**
- Per-minute pricing calculation
- Wallet balance system
- Credit card integration (Stripe)
- Ride receipts and invoices
- Promotional codes and discounts
- Ride history with cost breakdown

**Safety Features:**
- In-app safety guidelines
- Emergency contact button
- Report damaged scooter
- Photo documentation
- Helmet reminders

#### Technical Implementation (React Native)
```javascript
// Key technologies and libraries used:
• React Navigation for screen routing
• Redux for state management
• Google Maps React Native for mapping
• React Native Camera for QR scanning
• Stripe SDK for payments
• WebSocket for real-time updates
• AsyncStorage for local data
• React Native Permissions for location access
```

### 2. Administrative Dashboard

#### Fleet Management
**Scooter Management:**
- Add/remove scooters from fleet
- Scooter status monitoring (available, in-use, maintenance, damaged)
- Battery level tracking
- Location tracking (real-time map)
- Maintenance scheduling
- Scooter assignment to zones
- Bulk operations

**Zone Management:**
- Create operating zones (geofencing)
- No-parking zones
- Restricted areas
- Pricing zones (different rates per area)
- Service area boundaries
- Zone capacity limits

**Real-Time Monitoring:**
- Live scooter map with filters
- Active rides tracking
- Scooter availability heat map
- Battery level alerts
- Maintenance alerts
- Theft/movement alerts

#### User Management
**Customer Management:**
- User list with search and filters
- User profile viewing
- Ride history per user
- Balance management
- Account suspension/activation
- Support ticket management

**Staff Management:**
- Admin and operator accounts
- Role-based permissions
- Activity logs
- Team assignment for maintenance

#### Financial Management
**POS & Accounting System:**
- Transaction history
- Revenue reports (daily, weekly, monthly)
- Payment reconciliation
- Refund processing
- Promotional campaign management
- Pricing configuration
- Tax calculation
- Balance adjustments

**Analytics:**
- Revenue dashboards
- Ride statistics
- User engagement metrics
- Fleet utilization rates
- Popular routes and zones
- Peak usage times
- Customer retention metrics

### 3. Backend API & Business Logic

#### Core API Endpoints
```
Authentication:
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout

Scooters:
GET    /api/scooters/nearby        → Find scooters near user
GET    /api/scooters/{id}          → Get scooter details
POST   /api/scooters/{id}/unlock   → Start ride
POST   /api/scooters/{id}/lock     → End ride
POST   /api/scooters/{id}/reserve  → Reserve scooter

Rides:
GET    /api/rides                  → User ride history
GET    /api/rides/{id}             → Ride details
POST   /api/rides/{id}/pause       → Pause active ride
POST   /api/rides/{id}/end         → End ride

Payments:
GET    /api/wallet/balance         → Get wallet balance
POST   /api/wallet/topup           → Add funds
POST   /api/payments/methods       → Payment methods
POST   /api/payments/charge        → Process payment

Admin:
GET    /api/admin/scooters         → All scooters
PUT    /api/admin/scooters/{id}    → Update scooter
GET    /api/admin/analytics        → Dashboard analytics
GET    /api/admin/transactions     → Financial reports
```

#### Advanced Features Implemented

**Geofencing System:**
```php
// Check if location is within operating zone
function isWithinOperatingZone($latitude, $longitude) {
    $point = new Point($latitude, $longitude);
    $zones = Zone::where('active', true)->get();
    
    foreach ($zones as $zone) {
        if ($zone->polygon->contains($point)) {
            return true;
        }
    }
    return false;
}
```

**Distance Calculation:**
```php
// Haversine formula for accurate distance
function calculateDistance($lat1, $lon1, $lat2, $lon2) {
    $earthRadius = 6371; // km
    
    $dLat = deg2rad($lat2 - $lat1);
    $dLon = deg2rad($lon2 - $lon1);
    
    $a = sin($dLat/2) * sin($dLat/2) +
         cos(deg2rad($lat1)) * cos(deg2rad($lat2)) *
         sin($dLon/2) * sin($dLon/2);
    
    $c = 2 * atan2(sqrt($a), sqrt(1-$a));
    $distance = $earthRadius * $c;
    
    return $distance * 1000; // convert to meters
}
```

**Pricing Calculation:**
```php
// Dynamic pricing based on time and zone
function calculateRideCost($ride) {
    $duration = $ride->duration_minutes;
    $zone = $ride->start_zone;
    
    $baseFare = $zone->base_fare;
    $perMinuteRate = $zone->per_minute_rate;
    
    $cost = $baseFare + ($duration * $perMinuteRate);
    
    // Apply surge pricing during peak hours
    if ($this->isPeakHour()) {
        $cost *= 1.5;
    }
    
    return round($cost, 2);
}
```

### 4. IoT Integration

#### Scooter Hardware Communication

**Lock/Unlock System:**
- Send HTTP/MQTT commands to scooter
- Receive confirmation from hardware
- Queue-based command processing
- Retry mechanism for failed commands
- Timeout handling

**Real-Time GPS Tracking:**
- Scooter sends GPS coordinates every 10-30 seconds
- Location updates stored in database
- Real-time broadcast to admin dashboard
- Historical route tracking

**Status Monitoring:**
- Battery level reporting
- Motion detection (anti-theft)
- Diagnostic information
- Error reporting

#### Queue Job Implementation
```php
class UnlockScooterJob implements ShouldQueue
{
    protected $scooterId;
    
    public function handle()
    {
        $scooter = Scooter::find($this->scooterId);
        
        // Send unlock command to IoT device
        $response = Http::timeout(10)
            ->post($scooter->iot_endpoint . '/unlock', [
                'device_id' => $scooter->device_id,
                'command' => 'unlock'
            ]);
        
        if ($response->successful()) {
            $scooter->update(['status' => 'in_use']);
            event(new ScooterUnlocked($scooter));
        } else {
            // Retry logic
            $this->release(30); // retry after 30 seconds
        }
    }
}
```

---

## 💡 Key Technical Challenges & Solutions

### Challenge 1: Real-Time Scooter Location Updates
**Problem:** Thousands of scooters sending GPS data every 10 seconds  
**Solution:**
- Implemented Redis for caching latest locations
- Database writes batched every 60 seconds
- WebSocket broadcasting for admin dashboard
- Spatial indexing for fast nearby queries

### Challenge 2: IoT Reliability
**Problem:** Unreliable network connections with scooters  
**Solution:**
- Queue-based command system with retries
- Timeout handling and fallback mechanisms
- Status confirmation from hardware
- Local caching on mobile app

### Challenge 3: Geofencing Accuracy
**Problem:** GPS drift causing incorrect zone detection  
**Solution:**
- Implemented polygon-based geofencing
- Buffered zone boundaries
- Multiple GPS reading validation
- User warnings before leaving service area

### Challenge 4: Payment Processing
**Problem:** Handling interrupted rides and payment failures  
**Solution:**
- Wallet system for pre-funding
- Automatic retry for failed charges
- Manual reconciliation tools
- Grace period for negative balance

### Challenge 5: Mobile App Performance
**Problem:** Battery drain from constant GPS tracking  
**Solution:**
- Adaptive GPS polling (more frequent during rides)
- Background location updates optimization
- Smart wake locks
- Efficient map rendering

### Challenge 6: Cross-Platform Development
**Problem:** Different iOS and Android behaviors  
**Solution:**
- React Native for shared codebase (95%)
- Platform-specific code only when necessary
- Thorough testing on both platforms
- Native module bridging for camera and maps

---

## 📊 System Performance

### Mobile App Metrics
- **App Size:** 45MB (iOS), 38MB (Android)
- **Startup Time:** < 2 seconds
- **Battery Usage:** < 5% per hour during ride
- **Crash Rate:** < 0.1%
- **GPS Accuracy:** ±10 meters

### Backend Performance
- **API Response Time:** < 200ms average
- **GPS Updates Processing:** 10,000+ per minute
- **Concurrent Rides:** 1,000+ simultaneous rides
- **Database Queries:** < 50ms for 95% of queries
- **Uptime:** 99.8%

### Business Metrics
- **Fleet Size:** 500+ scooters deployed
- **Daily Rides:** 2,000+ rides per day
- **User Base:** 10,000+ registered users
- **Average Ride Duration:** 15 minutes
- **Average Ride Distance:** 2.5 km

---

## 🎓 Skills & Technologies Gained

### Mobile Development
- **React Native:**
  - Cross-platform app development
  - Native module integration
  - Performance optimization
  - App Store and Play Store deployment

- **Mobile-Specific:**
  - GPS and location services
  - QR code scanning
  - Push notifications
  - Background task processing
  - Camera integration

### IoT Integration
- **Hardware Communication:**
  - HTTP/MQTT protocols
  - Command-response patterns
  - Queue-based job processing
  - Real-time data streaming

- **Challenges Solved:**
  - Unreliable network handling
  - Command timeout management
  - Status synchronization

### Geospatial Features
- **Google Maps API:**
  - Map rendering and customization
  - Marker clustering
  - Route calculation
  - Distance matrix API
  - Geofencing with polygons

- **Spatial Calculations:**
  - Haversine formula for distance
  - Polygon containment checks
  - Route optimization
  - Heat maps

### Payment Integration
- **Stripe Integration:**
  - Payment method management
  - Charge processing
  - Subscription handling
  - Webhook processing
  - PCI compliance

### Business Logic
- **Mobility Operations:**
  - Fleet management strategies
  - Dynamic pricing models
  - Zone-based operations
  - Maintenance scheduling

---

## 🚀 Deployment & Publishing

### Mobile App Publishing

**iOS App Store:**
- Apple Developer account setup
- App Store Connect configuration
- TestFlight beta testing
- Review process and approval
- Production release

**Google Play Store:**
- Play Console setup
- Closed beta testing
- Open beta testing
- Review and publication
- Staged rollout

### Backend Deployment
- **Server:** Linux VPS with Nginx
- **Database:** MySQL with replication
- **Caching:** Redis cluster
- **Queue Workers:** Supervisor process management
- **SSL:** Let's Encrypt certificates
- **Monitoring:** Server monitoring and alerts

---

## 🏆 Project Outcomes

### Technical Achievements
✅ Successfully published on both iOS and Android stores  
✅ Integrated with physical IoT hardware  
✅ Built complete end-to-end mobility platform  
✅ Implemented complex geofencing system  
✅ Achieved 99.8% uptime since launch  

### Business Impact
✅ Platform deployed commercially in Algeria  
✅ Processing thousands of daily rides  
✅ Generated recurring revenue stream  
✅ Enabled expansion to multiple cities  
✅ Created jobs for maintenance and operations staff  

### Personal Growth
✅ First complete mobile app deployment (iOS + Android)  
✅ First IoT integration project  
✅ Advanced geospatial programming skills  
✅ Cross-platform development expertise  
✅ End-to-end project ownership experience  

---

## 🔮 Future Enhancements

Potential features for next phase:
- Electric bike integration
- Ride-sharing features
- Subscription plans
- Carbon footprint tracking
- AI-based demand prediction
- Battery swapping system
- Integration with public transit

---

## 📞 Project Inquiries

Interested in this IoT mobility project?

📧 **Email:** kareem.mohamed.swd@gmail.com  
🔗 **LinkedIn:** [kareem-mohamed-b3b676261](https://linkedin.com/in/kareem-mohamed-b3b676261)

---

[← Back to All Projects](../PROJECTS.md) | [View Next Project →](./on-my-way-therapy.md)
