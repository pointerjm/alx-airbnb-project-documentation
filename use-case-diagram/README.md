# Airbnb Clone Backend Use Case Diagram

## Overview
This document describes the **use case diagram** for the Airbnb Clone backend.  
It visualizes the interactions between key actors (**Guests, Hosts, Admins, and the System**) and the core functionalities of the system.  

The diagram captures essential user interactions, including:

- User registration  
- Property booking  
- Payment processing  
- Administrative tasks  

---

## Actors
- **Guest:** A user who searches for, books, and reviews properties.  
- **Host:** A user who creates, manages, and responds to reviews for property listings.  
- **Admin:** A system administrator who manages users, listings, bookings, and payments.  
- **System:** The Airbnb Clone backend, handling automated processes like notifications and payment processing.  

---

## Key Use Cases

### 1. User Management
- **Register:** Guests and Hosts can sign up for an account.  
- **Login:** Guests, Hosts, and Admins authenticate using email/password or OAuth.  
- **Update Profile:** Users (Guests and Hosts) manage their profile details.  

### 2. Property Listings Management
- **Create Listing:** Hosts add new property listings.  
- **Edit/Delete Listing:** Hosts update or remove their listings.  

### 3. Search and Filtering
- **Search Properties:** Guests search for properties by location, price, number of guests, and amenities.  
- **Filter Results:** Guests apply filters to refine search results.  

### 4. Booking Management
- **Create Booking:** Guests book a property for specific dates.  
- **Cancel Booking:** Guests or Hosts cancel bookings based on policies.  
- **View Booking Status:** Guests and Hosts check the status of bookings.  

### 5. Payment Integration
- **Process Payment:** Guests make payments via integrated gateways (e.g., Stripe, PayPal).  
- **Receive Payout:** Hosts receive automatic payouts after booking completion.  

### 6. Reviews and Ratings
- **Submit Review:** Guests leave reviews and ratings for properties.  
- **Respond to Review:** Hosts reply to guest reviews.  

### 7. Notifications System
- **Send Notification:** The system sends email or in-app notifications for booking updates, cancellations, and payments.  

### 8. Admin Dashboard
- **Manage Users:** Admins monitor and manage user accounts.  
- **Manage Listings:** Admins oversee property listings.  
- **Manage Bookings:** Admins handle booking disputes or issues.  
- **Manage Payments:** Admins monitor payment transactions.  

---

## Diagram Location
The use case diagram is available as a **PNG file** in the **`use-case-diagram/`** directory of the GitHub repository: **alx-airbnb-project-documentation**.
