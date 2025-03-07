# Tour Guide Scheduling Automation - Project Design Document

## 1. Executive Summary

This document outlines the technical design for the Tour Guide Scheduling Automation system for Lafayette College's Ambassadors of Lafayette program. The system will automate the scheduling process for campus tours, notify tour guides via Slack, track guide performance metrics, and manage specialty tour requirements.

## 2. System Architecture

### 2.1 Technology Stack

**Programming Language: Python**
- Rationale: Python offers excellent data processing capabilities, rich libraries for scheduling algorithms, and straightforward integration with external APIs like Slack.
- Additional benefits: Readable codebase for future maintenance, extensive documentation, and wide adoption in automation systems.

**Application Framework: Django**
- Provides a robust web interface for administrators and tour guides
- Includes built-in authentication and user management
- Offers an admin panel that can be customized for specific needs
- Supports database ORM for efficient data operations

**Database: PostgreSQL**
- Relational database for storing tour guide profiles, availability, tour assignments, and performance metrics
- Provides robust transaction support for scheduling operations
- Supports complex queries needed for the assignment algorithm

**Frontend: Django Templates + Tailwind CSS**
- Simple, responsive interface for administrators and tour guides
- Minimal JavaScript for enhanced user experience where needed
- Mobile-friendly design for on-the-go access

**Deployment: Docker + Nginx**
- Containerized deployment for consistency across environments
- Nginx as a reverse proxy for serving the application
- Can be hosted on college infrastructure or cloud services

**External Services Integration:**
- Slack API for notifications
- G Suite/Microsoft Office API for calendar integration (optional)
- Potential Slate CRM integration (pending approval)

### 2.2 System Components

```
┌─────────────────────────────────┐
│                                 │
│  Web Interface                  │
│  - Admin Dashboard              │
│  - Tour Guide Portal            │
│  - Office Worker Interface      │
│                                 │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│                                 │
│  Application Core               │
│  - User Authentication          │
│  - Data Management              │
│  - Scheduling Logic             │
│  - Reporting Engine             │
│                                 │
└───────────────┬─────────────────┘
                │
                ▼
┌─────────────────────────────────┐
│                                 │
│  Service Integrations           │
│  - Slack Notification Service   │
│  - Excel/CSV Import Service     │
│  - External API Connectors      │
│                                 │
└─────────────────────────────────┘
```

## 3. Database Schema

### 3.1 Core Tables

**Users**
- user_id (PK)
- username
- email
- password (hashed)
- role (admin, guide, office worker)
- create_date
- last_login

**TourGuides**
- guide_id (PK)
- user_id (FK)
- classification (experienced, regular, new)
- join_date
- total_tours_given
- specialty_tours_given
- absences
- excused_absences
- is_active

**Availability**
- availability_id (PK)
- guide_id (FK)
- day_of_week
- time_slot
- is_available
- effective_date
- expiration_date

**Tours**
- tour_id (PK)
- date
- time_slot
- visitor_count
- families_count
- special_accommodations
- is_specialty_tour
- specialty_type
- status (scheduled, completed, canceled)

**Assignments**
- assignment_id (PK)
- tour_id (FK)
- guide_id (FK)
- assignment_type (primary, backup, co-tour, evaluation)
- is_family_breaker
- status (assigned, confirmed, completed, absent)
- notification_sent
- check_in_time

**Requirements**
- requirement_id (PK)
- guide_id (FK)
- requirement_type
- target_count
- completed_count
- last_updated

### 3.2 Support Tables

**ExcuseRequests**
- request_id (PK)
- guide_id (FK)
- date
- time_slot
- reason
- status (pending, approved, denied)
- reviewed_by

**FeedbackRecords**
- feedback_id (PK)
- assignment_id (FK)
- rating
- comments
- submitted_date

**SystemMetrics**
- metric_id (PK)
- date
- scheduling_time
- conflicts_count
- manual_overrides
- system_uptime

## 4. Component Designs

### 4.1 Scheduling Engine

The scheduling engine is the core component responsible for matching available tour guides with tour requirements.

**Algorithm Approach:**
1. Process daily visitor counts to determine required number of guides
2. Filter available guides based on availability data
3. Apply weighted matching considering:
   - Guide classification
   - Specialty tour requirements
   - Tour count balance
   - Recent assignments
4. Generate optimal assignments with backup guides for critical tours
5. Provide manual override capabilities

**Implementation Details:**
- Use Python's PuLP or OR-Tools for constraint satisfaction problem solving
- Implement caching for performance optimization
- Run daily scheduling batch process with option for on-demand updates
- Maintain audit log of all scheduling decisions

### 4.2 Slack Integration Service

**Features:**
- Automated notifications to tour guides about assignments
- Configurable message templates
- Delivery scheduling and reminders
- Receipt confirmation tracking

**Implementation Details:**
- Use Slack's Bolt Python framework for reliable API integration
- Implement message queuing for reliability
- Store message templates in database for easy customization
- Log all notification attempts and delivery status

### 4.3 Data Import/Export Service

**Features:**
- Import tour guide availability from Excel/CSV
- Export schedules to various formats (Excel, PDF, CSV)
- Import visitor registration data
- Backup and restore capabilities

**Implementation Details:**
- Use pandas for efficient data processing
- Implement validation rules for imported data
- Provide error handling and reporting for failed imports
- Support scheduled and on-demand imports

### 4.4 Tracking and Metrics Module

**Features:**
- Tour guide performance dashboard
- Completion tracking for specialty tour requirements
- Attendance and reliability metrics
- System performance monitoring

**Implementation Details:**
- Use Django ORM for complex metric queries
- Implement caching for dashboard performance
- Schedule regular metric calculation jobs
- Export capabilities for reporting

## 5. User Interfaces

### 5.1 Administrative Dashboard

**Key Features:**
- Schedule overview with filtering and sorting
- Manual assignment adjustment interface
- Tour guide management portal
- System status monitoring
- Configuration settings

**Implementation:**
- Responsive design with Tailwind CSS
- Role-based access control
- Real-time updates where possible
- Advanced filtering and search capabilities

### 5.2 Tour Guide Portal

**Key Features:**
- Personal schedule view
- Availability management interface
- Excuse submission form
- Performance metrics and requirements progress
- Notification preferences

**Implementation:**
- Mobile-optimized interface
- Simplified navigation
- Clear visual indicators for requirements progress
- Integration with campus authentication (if available)

### 5.3 Office Worker Interface

**Key Features:**
- Daily schedule view
- Quick check-in/out interface
- Visitor count updates
- Special accommodation flags

**Implementation:**
- Simplified, task-focused design
- Touch-friendly interface for tablet use
- Minimal clicks for common operations
- Clear visual indicators for guide status

## 6. Security Considerations

### 6.1 Data Protection

- All user authentication through secure methods
- Personal data encryption in database
- Role-based access control
- Session timeout and management
- Regular security audits

### 6.2 Integration Security

- API key rotation and secure storage
- Rate limiting on external service calls
- Secure webhook endpoints for incoming data
- HTTPS for all communications
- Input validation and sanitization

## 7. Deployment Strategy

### 7.1 Development Environment

- Local development using Docker Compose
- Git repository for version control
- CI/CD pipeline for automated testing
- Development, staging, and production environments

### 7.2 Production Environment

**Option 1: College Infrastructure**
- Deploy on existing college servers
- Integrate with college authentication
- Regular backups to college storage systems

**Option 2: Cloud Deployment**
- AWS/GCP/Azure deployment
- Containerized application with orchestration
- Managed database service
- Automated scaling based on demand

### 7.3 Maintenance Plan

- Regular database backups
- Scheduled system updates
- Monitoring and alerting setup
- Documentation for operational procedures
- Knowledge transfer to IT staff

## 8. Implementation Timeline

### Phase 1: Core System (Weeks 1-3)
- Database schema implementation
- Basic scheduling algorithm
- Administrative interface
- Data import/export functionality

### Phase 2: Integration & Enhancement (Weeks 4-6)
- Slack API integration
- Advanced scheduling rules
- Tour guide portal
- Reporting functionality

### Phase 3: Testing & Refinement (Weeks 7-8)
- User acceptance testing
- Performance optimization
- Security review
- Documentation completion

### Phase 4: Deployment & Training (Weeks 9-10)
- Production deployment
- Administrator training
- Tour guide onboarding
- Monitoring setup

## 9. Future Enhancements

- Mobile application for tour guides
- Analytics dashboard for program optimization
- Visitor feedback collection system
- Calendar integration with campus event system
- Machine learning for predictive assignment optimization
- Integration with campus mapping for tour route optimization

## 10. Resource Requirements

### Technical Resources
- Development environment setup
- Production hosting environment
- Slack API access tokens
- Database server
- Backup storage

### Human Resources
- Backend developer (Python/Django)
- Frontend developer (HTML/CSS/JavaScript)
- DevOps engineer (part-time)
- QA tester
- Project manager