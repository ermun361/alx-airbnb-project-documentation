# Backend Requirement Specifications

This document outlines the functional and non-functional requirements for the core backend features of the ALX Airbnb project. It includes API endpoint definitions, data formats, validation rules, and performance criteria to guide the development process.


## 1. User Authentication Service

This service handles user registration, login, and session management.

### 1.1. User Registration

*   **Description:** Allows a new user to create an account.
*   **API Endpoint:** `POST /api/auth/register`
*   **Input (Request Body):**
    ```json
    {
      "firstName": "John",
      "lastName": "Doe",
      "email": "johndoe13@gmail.com",
      "password": "Password123!"
    }
    ```
*   **Validation Rules:**
    *   All fields are mandatory.
    *   `email` must be a valid email format and must be unique in the `Users` database.
    *   `password` must be at least 8 characters long, containing at least one uppercase letter, one lowercase letter, one number, and one special character.
*   **Output (Success `201 Created`):**
    ```json
    {
      "status": "success",
      "message": "User registered successfully.",
      "data": {
        "userId": "c3a4b1d2-e5f6-7890-1234-567890abcdef"
      }
    }
    ```
*   **Output (Error Responses):**
    *   `400 Bad Request`: If validation fails (e.g., invalid email, weak password).
    *   `409 Conflict`: If the email address already exists.
*   **Performance Criteria:** The API should respond within **500ms** under normal load.
*   **Security:** Passwords must be securely hashed using a strong algorithm (e.g., bcrypt) before being stored.

### 1.2. User Login

*   **Description:** Authenticates a user and provides an access token.
*   **API Endpoint:** `POST /api/auth/login`
*   **Input (Request Body):**
    ```json
    {
      "email": "johndoe13@gmail.com",
      "password": "Password123!"
    }
    ```
*   **Validation Rules:**
    *   `email` and `password` are mandatory.
*   **Output (Success `200 OK`):**
    ```json
    {
      "status": "success",
      "message": "Login successful.",
      "data": {
        "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
        "user": {
            "userId": "c3a4b1d2-e5f6-7890-1234-567890abcdef",
            "firstName": "John"
        }
      }
    }
    ```
*   **Output (Error Response):**
    *   `401 Unauthorized`: If credentials are incorrect.
*   **Performance Criteria:** Response time should be under **300ms**.
*   **Security:** The system must use JSON Web Tokens (JWT) for session management. The token should contain the user's ID and role and have a defined expiration period.


## 2. Property Management Service

This service allows hosts to create, update, and manage their property listings.

### 2.1. Create a New Property Listing

*   **Description:** Allows an authenticated host to add a new property to the platform.
*   **API Endpoint:** `POST /api/properties`
*   **Input (Request Body):**
    ```json
    {
      "title": "GTC Apartment",
      "description": "A beautiful apartment in the heart of Nairobi city.",
      "location": "Westlands, Nairobi, Kenya",
      "pricePerNight": 150.00,
      "maxGuests": 4,
      "amenities": ["Wi-Fi", "Kitchen", "Air Conditioning"]
    }
    ```
*   **Validation Rules:**
    *   All fields are mandatory.
    *   `pricePerNight` and `maxGuests` must be positive numbers.
    *   `amenities` must be an array of strings.
*   **Output (Success `201 Created`):** Returns the full object of the newly created property, including its system-generated ID.
    ```json
    {
      "status": "success",
      "data": {
        "propertyId": "p_987xyz",
        "hostId": "c3a4b1d2-e5f6-7890-1234-567890abcdef",
        "title": "Cozy Downtown Apartment",
        ...
      }
    }
    ```
*   **Output (Error Response):**
    *   `400 Bad Request`: If validation fails.
    *   `401 Unauthorized`: If the user is not authenticated.
    *   `403 Forbidden`: If the authenticated user is not a host.
*   **Performance Criteria:** Response time should be under **600ms**.
*   **Security:** This endpoint must be protected. The `hostId` should be extracted from the user's authentication token, not passed in the request body.



## 3. Booking System

This service handles the creation and management of property bookings.

### 3.1. Create a Booking

*   **Description:** Allows an authenticated guest to book a property for a specified date range.
*   **API Endpoint:** `POST /api/bookings`
*   **Input (Request Body):**
    ```json
    {
      "propertyId": "p_987xyz",
      "checkInDate": "2024-12-20",
      "checkOutDate": "2024-12-25"
    }
    ```
*   **Validation Rules:**
    *   All fields are mandatory.
    *   `propertyId` must correspond to an existing property.
    *   `checkInDate` and `checkOutDate` must be valid dates. `checkOutDate` must be after `checkInDate`.
    *   The system must check for booking conflicts (i.e., the requested dates must not overlap with any existing confirmed bookings for that property).
*   **Output (Success `201 Created`):**
    ```json
    {
      "status": "success",
      "message": "Booking created successfully. Awaiting payment.",
      "data": {
        "bookingId": "b_123abc",
        "propertyId": "p_987xyz",
        "guestId": "g_guest_user_id",
        "checkInDate": "2024-12-20",
        "checkOutDate": "2024-12-25",
        "totalPrice": 750.00,
        "status": "pending_payment"
      }
    }
    ```
*   **Output (Error Responses):**
    *   `400 Bad Request`: Invalid input data (e.g., checkout is before check-in).
    *   `401 Unauthorized`: User is not logged in.
    *   `404 Not Found`: The specified `propertyId` does not exist.
    *   `409 Conflict`: The property is already booked for the requested dates.
*   **Performance Criteria:** Response time, including the conflict check, should be under **800ms**.
*   **Security:** The `guestId` must be derived from the authenticated user's token. The price calculation must be performed on the backend based on the stored property price to prevent tampering.