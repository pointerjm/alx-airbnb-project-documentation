# Airbnb Clone Backend Requirement Specifications

## Overview
This document outlines the **technical and functional requirements** for three key backend features of the Airbnb Clone project: **User Authentication, Property Management, and Booking System**.  
Each feature includes API endpoints, input/output specifications, validation rules, and performance criteria to ensure a **secure, scalable, and efficient system**.

---

## 1. User Authentication

### Functional Requirements
- **Purpose:** Allow users (Guests, Hosts, Admins) to register, log in, and manage authentication securely.
- **Features:**
  - User registration with email/password or OAuth (Google, Facebook)
  - User login with email/password or OAuth
  - JWT-based session management with role-based access control (RBAC)

### Technical Requirements
- **Database:**
  - Table: `Users`
  - Columns: `id` (UUID, PK), `email` (varchar, unique), `password` (hashed), `role` (enum: Guest, Host, Admin), `created_at` (timestamp)
- **Authentication:** JWT for session tokens, stored in HTTP-only cookies or Authorization header
- **Security:** Passwords hashed with bcrypt; OAuth via third-party providers
- **Third-Party Services:** OAuth providers (Google, Facebook) integration

### API Endpoints

**POST /api/auth/register**  
- **Description:** Register a new user  
- **Input:** `{ "email": string, "password": string, "role": "Guest" | "Host" }`  
- **Validation:** email format, unique; password ≥ 8 chars; role must be Guest or Host  
- **Output:** Success `{ "message": "User registered successfully", "user_id": UUID }` (201)  
- **Performance:** Response < 500ms for 95% of requests  

**POST /api/auth/login**  
- **Description:** Authenticate user and issue JWT  
- **Input:** `{ "email": string, "password": string }`  
- **Output:** Success `{ "message": "Login successful", "token": JWT }` (200)  
- **Performance:** Response < 300ms  

**POST /api/auth/oauth**  
- **Description:** Authenticate via OAuth provider  
- **Input:** `{ "provider": "Google" | "Facebook", "access_token": string }`  
- **Output:** Success `{ "message": "OAuth login successful", "token": JWT }` (200)  
- **Performance:** Response < 600ms  

### Validation Rules
- Sanitize all inputs to prevent SQL injection & XSS
- JWT expires after 24h; refreshable
- Rate limiting: max 5 failed login attempts per IP / 15 min

### Performance Criteria
- Handle 1,000 concurrent authentication requests with < 1% error rate
- Index `email` and `id` for fast queries
- Cache JWT validation in Redis for 5s

---

## 2. Property Management

### Functional Requirements
- **Purpose:** Enable Hosts to create, update, and delete property listings
- **Features:**
  - Create listings (title, description, location, price, amenities, availability)
  - Edit or delete listings
  - Store property images in cloud storage (AWS S3)

### Technical Requirements
- **Database:**  
  Table: `Properties`  
  Columns: `id` (UUID, PK), `host_id` (UUID, FK), `title` (varchar), `description` (text), `location` (varchar), `price` (decimal), `amenities` (JSON), `availability` (JSON), `created_at` (timestamp)  
- **File Storage:** AWS S3 for images, URLs stored in DB  
- **Authorization:** Only authenticated Hosts can manage their listings

### API Endpoints

**POST /api/properties**  
- Create a new property listing  
- Input: title, description, location, price, amenities, availability  
- Output: Success `{ "message": "Property created", "property_id": UUID }` (201)  
- Performance: < 500ms  

**PUT /api/properties/:id**  
- Update an existing listing  
- Validation: must belong to authenticated Host  
- Output: Success `{ "message": "Property updated" }` (200)  
- Performance: < 400ms  

**DELETE /api/properties/:id**  
- Delete a listing  
- Validation: must belong to authenticated Host  
- Output: Success `{ "message": "Property deleted" }` (200)  
- Performance: < 300ms  

### Validation Rules
- Inputs sanitized for security  
- Only listing owner can edit/delete (RBAC enforced)  
- Availability dates cannot overlap existing bookings  

### Performance Criteria
- Support 500 concurrent listing creations with <1% error rate  
- Index `host_id` and `id`  
- Cache frequently accessed listings in Redis for 10 min

---

## 3. Booking System

### Functional Requirements
- **Purpose:** Allow Guests to book properties and manage bookings  
- **Features:**
  - Create bookings for specific dates (prevent double bookings)  
  - Cancel bookings per policy  
  - Track booking statuses: pending, confirmed, canceled, completed

### Technical Requirements
- **Database:**  
  Table: `Bookings`  
  Columns: `id` (UUID, PK), `guest_id` (UUID, FK), `property_id` (UUID, FK), `start_date` (date), `end_date` (date), `status` (enum), `created_at` (timestamp)  
- **Authorization:** Authenticated Guests create bookings; Guests/Hosts cancel per policy  
- **Validation:** Dates must be available; no conflicts

### API Endpoints

**POST /api/bookings**  
- Create a booking  
- Input: property_id, start_date, end_date  
- Output: Success `{ "message": "Booking created", "booking_id": UUID }` (201)  
- Performance: < 500ms  

**PATCH /api/bookings/:id/cancel**  
- Cancel booking  
- Output: Success `{ "message": "Booking canceled" }` (200)  
- Performance: < 300ms  

**GET /api/bookings/:id**  
- Retrieve booking details  
- Output: Success `{ "booking_id": UUID, "property_id": UUID, "start_date": "...", "end_date": "...", "status": "..." }` (200)  
- Performance: < 200ms  

### Validation Rules
- Prevent double bookings  
- Enforce cancellation policies (e.g., no cancellations within 24h)  
- Sanitize all inputs

### Performance Criteria
- Handle 1,000 concurrent bookings with <1% error rate  
- Index `property_id`, `start_date`, `end_date`  
- Cache booking statuses in Redis for 5 min

---

## Repository Location
The detailed requirement specifications are stored in:  
**`requirements.md`** in the root directory of the GitHub repository: **alx-airbnb-project-documentation**
