# OrthoCareChain

OrthoCareChain is an enterprise-grade, role-based orthopedic healthcare management system designed to digitize and streamline patient visit management, medical reporting, prescription tracking, diagnostic image handling, and secure patient data storage.

---

## Executive Summary

OrthoCareChain provides a centralized platform for managing orthopedic patient workflows. The application ensures secure authentication, structured visit-based medical records, scalable cloud image storage, automated patient notifications, and AES-256-GCM encryption for sensitive patient data at rest.

The system is built using a layered backend architecture with secure JWT-based authentication and role-based authorization. It demonstrates best practices in REST API development, relational database modeling, field-level data encryption, and full-stack system design.

---

## System Architecture

```
Client (React Frontend)
        ↓
REST API Layer (Spring Boot Controllers)
        ↓
Business Logic Layer (Service Layer)
        ↓
Data Access Layer (Repository Layer - JPA / AttributeConverter - AES-256-GCM)
        ↓
Relational Database (MySQL - Encrypted sensitive columns)
```

Supporting Services

- Cloudinary (Medical Image Storage)
- Email Scheduler (Automated Appointment and Drug Reminders)
- iText (Server-side PDF Generation)

---

## Technology Stack

### Backend

- Java 17
- Spring Boot 3
- Spring Security
- JWT (Stateless Authentication)
- Spring Data JPA / Hibernate
- MySQL
- iText (PDF Generation)
- Cloudinary SDK
- JavaMail (Email Notifications)
- Maven

### Frontend

- React 18
- Vite
- Context API (Auth and Toast State)
- Native Fetch API (No external HTTP library)

---

## Security

### Authentication

- JWT-based stateless authentication
- BCrypt password hashing
- Role-based access control with Spring Security
- Method-level authorization using @PreAuthorize

### Patient Data Encryption

Fields encrypted at rest:

- Date of Birth
- Allergies
- Chronic Conditions
- Emergency Contact Name
- Emergency Contact Phone
- Home Address


---

## Functional Modules

### Authentication and Authorization

- JWT-based stateless authentication with access token and refresh token
- BCrypt password encryption
- Role-based access control for four user types
- Method-level security on all protected endpoints

### User Management

- Admin-controlled admin account creation
- Role-based self-registration for Doctor, Patient, and Pharmacy
- User activation and deactivation
- Safe cascaded user deletion respecting all foreign key constraints

### Visit-Based Report Management

- Each patient visit creates a unique medical report
- Diagnosis, severity level, treatment plan, and doctor notes per visit
- Visit-level medical history preserved and traceable
- Admin view for all system-wide reports

### Prescription Management

- Multiple prescriptions per visit report
- Dosage, frequency, duration, and instructions tracked per drug
- Controlled pharmacy access limited to prescription data only
- Prescription activation and deactivation support

### Medical Image Management

- Cloud-based image storage via Cloudinary
- Support for X-Ray, MRI, CT Scan, Ultrasound, and Other types
- Only image URLs and metadata stored in the database
- Doctor and Admin upload access with patient read access

### PDF Generation

- Downloadable visit-based medical reports generated server-side using iText
- Color-coded severity badges, teal section headers, prescription cards, and image metadata table
- Access-controlled so patients can only download their own reports

### Notification System

- Automated next-visit appointment reminders via email
- Automated drug intake reminders via Spring Scheduler

---

## Role Access Matrix

| Feature                  | Admin | Doctor | Patient | Pharmacy |
|--------------------------|:-----:|:------:|:-------:|:--------:|
| View all users           |  Yes  |   No   |   No    |    No    |
| Create admin account     |  Yes  |   No   |   No    |    No    |
| View all reports         |  Yes  |   No   |   No    |    No    |
| Create visit report      |  Yes  |  Yes   |   No    |    No    |
| View own reports         |   No  |  Yes   |  Yes    |    No    |
| Upload medical images    |  Yes  |  Yes   |   No    |    No    |
| Download PDF report      |  Yes  |  Yes   |  Yes    |    No    |
| View prescriptions       |  Yes  |  Yes   |  Yes    |   Yes    |
| Manage doctor profile    |   No  |  Yes   |   No    |    No    |
| Manage patient profile   |   No  |   No   |  Yes    |    No    |

---

## Database Relationships

```
User (1) ──── (1) Doctor
User (1) ──── (1) Patient

Doctor  (1) ──── (Many) Reports
Patient (1) ──── (Many) Reports

Report (1) ──── (Many) Prescriptions
Report (1) ──── (Many) MedicalImages
```

Deletion is handled in cascaded order to respect all foreign key constraints. Deleting a user first removes their reports along with all prescriptions and images, then removes the doctor or patient profile, and finally removes the user record.

---

## API Overview

| Controller    | Base Path           | Key Endpoints                                             |
|---------------|---------------------|-----------------------------------------------------------|
| Auth          | /api/auth           | POST /login, /register, /logout, /refresh-token           |
| Admin         | /api/admin          | GET /users, POST /users/create-admin, PUT /toggle-active  |
| Reports       | /api/reports        | POST /, GET /all, /my-reports, /{id}/download-pdf         |
| Doctors       | /api/doctors        | POST /profile, GET /profile/me, GET /                     |
| Patients      | /api/patients       | POST /profile, GET /profile/me, GET /                     |
| Images        | /api/images         | POST /upload/{reportId}, GET /report/{reportId}           |
| Prescriptions | /api/prescriptions  | GET /report/{id}, GET /pharmacy/patient/{id}              |

---

## Installation and Setup

### Clone Repository

```bash
git clone https://github.com/BaskaranV15/ortho.git
```

### Backend Setup

Step 1. Create a MySQL database named orthocarechain

Step 2. Open src/main/resources/application.properties and configure the following

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/orthocarechain
spring.datasource.username=your_mysql_username
spring.datasource.password=your_mysql_password

cloudinary.cloud-name=your_cloud_name
cloudinary.api-key=your_api_key
cloudinary.api-secret=your_api_secret

spring.mail.username=your_email
spring.mail.password=your_email_password

aes.secret.key=your_32_character_secret_key_here
```

Step 3. Run the backend

```bash
./mvnw spring-boot:run
```

Backend runs at http://localhost:8080

On first startup, default accounts are created automatically.

### Frontend Setup

```bash
cd ortho
npm install
npm run dev
```

Frontend runs at http://localhost:5173


## Future Enhancements

- Blockchain-based medical record verification for tamper-proof audit trails
- AI-based fracture detection integration from uploaded X-ray images
- Appointment scheduling and calendar system
- Full audit logging and compliance tracking
- Multi-hospital and multi-branch support
- Mobile application for patient access

---

## About the Name

The name OrthoCareChain refers to the chain of linked medical records within the system. A user account links to a doctor or patient profile. That profile links to visit reports. Each report links to prescriptions and medical images. This traceable chain of connected records is enforced at the database level through foreign key relationships and secured at the application level through role-based access control.
