# OrthoCareChain

OrthoCareChain is an enterprise-grade, role-based orthopedic healthcare management system designed to digitize and streamline patient visit management, medical reporting, prescription tracking, and diagnostic image handling.

---

## Executive Summary

OrthoCareChain provides a centralized platform for managing orthopedic patient workflows. The application ensures secure authentication, structured visit-based medical records, scalable image storage, and automated patient notifications.

The system is built using a layered backend architecture with secure JWT-based authentication and role-based authorization. It demonstrates best practices in REST API development, relational database modeling, and full-stack system design.

---

## System Architecture

Client (React Frontend)
        ↓
REST API Layer (Spring Boot Controllers)
        ↓
Business Logic Layer (Service Layer)
        ↓
Data Access Layer (Repository Layer - JPA)
        ↓
Relational Database (MySQL)

Supporting Services:
- Cloudinary (Image Storage)
- Email Scheduler (Automated Reminders)

---

## Technology Stack

### Backend
- Java 17
- Spring Boot
- Spring Security
- JWT (Stateless Authentication)
- Spring Data JPA
- MySQL
- Cloudinary
- Maven

### Frontend
- React
- React Router
- Axios

---

## Functional Modules

### Authentication & Authorization
- JWT-based stateless authentication
- BCrypt password encryption
- Role-based access control
- Method-level authorization

### User Management
- Admin-controlled account creation
- Doctor and Patient profile management
- User activation/deactivation

### Visit-Based Report Management
- Each patient visit creates a unique medical report
- Diagnosis and severity tracking
- Visit-level medical history preservation

### Prescription Management
- Multiple prescriptions per visit
- Dosage, frequency, duration, and instructions tracking
- Controlled pharmacy access to prescription data

### Medical Image Management
- Cloud-based image storage via Cloudinary
- Only image URLs stored in the database

### PDF Generation
- Downloadable visit-based medical reports

### Notification System
- Automated next-visit reminders
- Automated drug intake reminders

---

## Database Relationships

User (1) — (1) Doctor  
User (1) — (1) Patient  

Doctor (1) — (Many) Reports  
Patient (1) — (Many) Reports  

Report (1) — (Many) Prescriptions  
Report (1) — (Many) ReportImages  

---

## Installation & Setup

### Clone Repository

git clone https://github.com/your-username/orthocarechain.git

### Backend Setup

1. Configure database credentials in application.yml
2. Ensure MySQL server is running
3. Run:

mvn spring-boot:run

Backend runs at:
http://localhost:8080

### Frontend Setup

cd orthocarechain-frontend  
npm install  
npm start  

Frontend runs at:
http://localhost:3000

---

## Future Enhancements

- Blockchain-based medical record verification
- AI-based fracture detection integration
- Appointment scheduling system
- Audit logging and compliance tracking
- Multi-hospital support

---

## License

Developed for academic and educational purposes.
