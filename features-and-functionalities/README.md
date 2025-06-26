# 🏡 Airbnb Clone Backend - Features and Functionalities

##  Objective
This document outlines the backend features and functionalities for the Airbnb Clone project. It covers all critical system capabilities including authentication, property management, booking workflows, and payment processing.


##  Core Functionalities

### 1. User Management
- User Registration (guest/host role)
- Login with email/password (JWT)
- OAuth login (Google/Facebook)
- Profile updates

### 2. Property Listings Management
- Add/Edit/Delete Listings
- Include title, description, location, price, amenities, and availability
- Upload photos

### 3. Search & Filtering
- Search by location, price, amenities, guest capacity
- Pagination support

### 4. Booking System
- Booking creation with date validation
- Booking status: pending, confirmed, canceled, completed
- Cancel bookings

### 5. Payment Integration
- Stripe/PayPal for secure guest payments
- Host payouts after stay completion
- Multi-currency support

### 6. Reviews & Ratings
- Guests review listings
- Hosts can respond
- Reviews are linked to verified bookings

### 7. Notification System
- Email + in-app alerts for:
  - Booking confirmation
  - Payment status
  - Cancellation notices

### 8. Admin Dashboard
- Manage users, listings, bookings, and payments


##  Technical Stack and Requirements

- **Database**: PostgreSQL/MySQL
- **API**: REST (GraphQL optional)
- **Auth**: JWT + RBAC (guest/host/admin roles)
- **Storage**: Local for now (future: AWS S3/Cloudinary)
- **Email**: SendGrid or Mailgun
- **Payments**: Stripe or PayPal
- **Testing**: Pytest, Jest


##  Non-Functional Requirements

- **Scalability**: Modular architecture, load balancing
- **Security**: Data encryption, rate limiting, firewalls
- **Performance**: Redis caching, optimized DB queries
- **Testing**: Unit + integration test coverage

