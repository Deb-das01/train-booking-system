# Train Booking and Reservation System

## Overview

Welcome, Developer! You are tasked with building a **Train Booking and Reservation System** that enables users to search for trains, check seat availability, and make reservations. Before booking, users must log in, ensuring secure transactions. The system should efficiently handle simultaneous booking requests while preventing race conditions. Additionally, an admin panel should be provided for train management and seat allocation.

## Key Features

- **User Registration & Authentication**
- **JWT-secured access control**
- **Train search based on departure and destination cities**
- **Real-time seat availability updates**
- **Secure booking system with concurrency management**
- **Admin functionalities for train scheduling**
- **Error handling & data validation**

---

## Setup Instructions

### Prerequisites
Before setting up the project, ensure you have:

- [Node.js](https://nodejs.org/) (Version 14 or higher)
- [MySQL](https://www.mysql.com/)
- [Postman](https://www.postman.com/) (For API testing)

### Environment Configuration
Create a `.env` file in the project root with:

```bash
PORT=5000
DB_HOST=localhost
DB_USER=admin
DB_PASSWORD=yourpassword
DB_NAME=train_reservation_db
JWT_SECRET=super_secure_secret
ADMIN_API_KEY=your_admin_key
```

### Installation Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/train-booking-system.git
   cd train-booking-system
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Set up the MySQL database:
   ```sql
   CREATE DATABASE train_reservation_db;
   USE train_reservation_db;

   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       full_name VARCHAR(255) NOT NULL,
       email VARCHAR(255) UNIQUE NOT NULL,
       password VARCHAR(255) NOT NULL,
       role ENUM('customer', 'admin') DEFAULT 'customer',
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE trains (
       id INT AUTO_INCREMENT PRIMARY KEY,
       train_code VARCHAR(50) NOT NULL,
       origin VARCHAR(255) NOT NULL,
       destination VARCHAR(255) NOT NULL,
       total_seats INT NOT NULL,
       available_seats INT NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE reservations (
       id INT AUTO_INCREMENT PRIMARY KEY,
       user_id INT,
       train_id INT,
       seats_reserved INT NOT NULL,
       FOREIGN KEY (user_id) REFERENCES users(id),
       FOREIGN KEY (train_id) REFERENCES trains(id)
   );
   ```

### Running the Application
Start the server using:
```bash
npm start
```
By default, it runs on `http://localhost:5000`.

---

## API Endpoints

### **User Routes**

1. **Register a New User**
   - **POST** `/user/signup`
   - **Request Body:**
     ```json
     {
       "full_name": "Alice Johnson",
       "email": "alice@example.com",
       "password": "mypassword"
     }
     ```

2. **Login**
   - **POST** `/user/login`
   - **Request Body:**
     ```json
     {
       "email": "alice@example.com",
       "password": "mypassword"
     }
     ```

3. **Search Trains**
   - **GET** `/trains/search?origin=Bangalore&destination=Hyderabad`
   - **Response:**
     ```json
     {
       "available": true,
       "trainCount": 3,
       "trains": [
         {
           "trainCode": "EXP123",
           "availableSeats": 50
         }
       ]
     }
     ```

4. **Book Seats** (JWT Required)
   - **POST** `/booking/reserve`
   - **Request Body:**
     ```json
     {
       "trainId": 2,
       "seats": 3
     }
     ```
   - **Response:**
     ```json
     {
       "message": "Booking successful"
     }
     ```

5. **User's Bookings**
   - **GET** `/booking/history`
   - **Response:**
     ```json
     [
       {
         "reservation_id": 5,
         "train_code": "EXP123",
         "origin": "Bangalore",
         "destination": "Hyderabad",
         "seats_reserved": 3
       }
     ]
     ```

### **Admin Routes**

1. **Add a Train** (Admin API Key Required)
   - **POST** `/admin/addTrain`
   - **Headers:** `x-api-key: ADMIN_API_KEY`
   - **Request Body:**
     ```json
     {
       "trainCode": "RAJ111",
       "origin": "Delhi",
       "destination": "Mumbai",
       "totalSeats": 700,
       "availableSeats": 700
     }
     ```
   - **Response:**
     ```json
     {
       "message": "Train added successfully"
     }
     ```

2. **Update Seat Availability**
   - **PUT** `/admin/updateSeats/7`
   - **Headers:** `x-api-key: ADMIN_API_KEY`
   - **Request Body:**
     ```json
     {
       "totalSeats": 600,
       "availableSeats": 450
     }
     ```
   - **Response:**
     ```json
     {
       "message": "Seats updated successfully"
     }
     ```

---

## Technologies Used

- **Node.js & Express.js** - Backend Framework
- **MySQL** - Relational Database
- **JWT** - Secure Authentication
- **bcrypt.js** - Password Hashing

## Future Improvements

- **Frontend Development** (React.js or Vue.js)
- **Real-time Seat Selection Feature**
- **Payment Gateway Integration**

## Contributing
We welcome contributions! Fork the repo, make improvements, and submit a pull request.

---

**Developed by:** SWASTIDEB DAS | 2025

