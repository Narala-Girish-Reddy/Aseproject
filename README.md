[28-10-2026 10:49] Anil: # MediLink – Smart Blood & Medicine Emergency Network


## 📌 Project Overview

MediLink is a centralized, web-based emergency healthcare discovery platform designed to eliminate life-threatening delays during the "golden hour" of emergency medical care. When critical surgical emergencies, road trauma, or acute medical conditions occur, patients and families often struggle to locate compatible blood units or rare medicines due to data silos and reliance on manual phone inquiries.

MediLink directly bridges this gap by connecting patients, verified blood donors, volunteer dispatchers, and local pharmacies in a unified real-time network featuring location-based proximity matching powered by the Google Maps API.

---

## 👥 Team & Academic Details

* Course Name: Adaptive Software Engineering (ASE)
* Course Code: 24CI3201
* Academic Year: 2026–2027
* Project Guide: Bhavya Varma K

### Team Members

| S. No. | Name | Roll Number | Role / Module |
| :---: | :--- | :---: | :--- |
| 1 | A. Anil | 2420030130 | Emergency Blood Requests & Request Lifecycle Tracking |
| 2 | N. Girish Reddy | 2420030125 | User Authentication, Donor Profiles & Availability Engine |
| 3 | S. Sri Charan Reddy | 2420030127 | Medicine Finder, Pharmacy Services & Maps Integration |

---

## 🚀 Key Features

* Real-Time Blood Request Management: Create emergency blood requests with blood group, units required, hospital location, and urgency status.
* Proximity Donor Matching: Search and filter compatible donors by blood group and proximity using location coordinates.
* Medicine Finder: Instant search for essential medicines, stock availability, and 24/7 operating hours across verified pharmacies.
* Volunteer Response Network: Enable community volunteers to coordinate donor dispatch and transport logistics.
* Live Status Tracking: Real-time request state transitions (Open → Accepted → In-Progress → Completed).
* Interactive Geo-Mapping: Embedded Google Maps API to visualize routes and calculate shortest transit times.

---

## 🛠 Technology Stack

| Layer | Technology |
| :--- | :--- |
| Frontend | JSP (JavaServer Pages), HTML5, CSS3, JavaScript (ES6+), Bootstrap 5 |
| Backend | Java (Servlets, MVC Architecture) |
| Database | MySQL 8.0 |
| Database Connectivity| JDBC (Java Database Connectivity) |
| Application Server | Apache Tomcat 9.0 / 10.0 |
| External APIs | Google Maps JavaScript API, Places API, REST Web Services |
| Version Control & PM | Git, GitHub, Jira (Agile Scrum) |

---

## 🏗 System Architecture

               Patient / Hospital Staff / Donor
                              │
                              ▼
                 ┌──────────────────────────┐
                 │  Presentation Tier (UI)  │
                 │   JSP, HTML5, CSS3, JS   │
                 └────────────┬─────────────┘
                              │ HTTP Requests
                              ▼
                 ┌──────────────────────────┐
                 │    Controller Tier       │
                 │   Java Servlets / MVC    │
                 └────────────┬─────────────┘
                              │
            ┌─────────────────┴─────────────────┐
            ▼                                   ▼
┌───────────────────────┐           ┌───────────────────────┐
│     Business Layer    │           │   External Services   │
│  - Donor Matching     │           │  - Google Maps API    │
│  - Emergency Dispatch │           │  - Geocoding API      │
│  - Medicine Search    │           │  - Notification Bus   │
└───────────┬───────────┘           └───────────────────────┘
            │
            ▼
┌───────────────────────┐
│      Data Access      │
│      JDBC Driver      │
└───────────┬───────────┘
            │
            ▼
┌───────────────────────┐
│    MySQL Database     │
│   (Users, Inventory,  │
│    Requests, Donors)  │
└───────────────────────┘

---

## 🔄 Emergency Blood Request Lifecycle
[28-10-2026 10:49] Anil:  [1. Create Request] 
      │ (Blood Group, Units, Hospital, Urgency)
      ▼
 [2. Matrix Scan] 
      │ (Query Proximity via Google Maps API)
      ▼
 [3. Instant Alert] 
      │ (Notify eligible registered donors & volunteers)
      ▼
 [4. Accepted] 
      │ (A donor or volunteer claims the emergency ticket)
      ▼
 [5. In-Progress] 
      │ (Sample dispatch & transit underway)
      ▼
 [6. Completed] 
      │ (Unit verified & ticket closed in database)

---

## 📂 Project Directory Structure

MediLink/
├── src/
│   └── main/
│       └── java/
│           └── com/medilink/
│               ├── config/          # DB Connection pool & environment configs
│               ├── controller/      # Servlets (AuthServlet, RequestServlet, etc.)
│               ├── dao/             # Data Access Objects (UserDAO, DonorDAO, etc.)
│               ├── model/           # Java Beans (User, BloodRequest, Medicine, etc.)
│               └── util/            # Helper classes (GeoLocationUtil, Mailer)
├── WebContent/ / src/main/webapp/
│   ├── assets/
│   │   ├── css/                     # Custom stylesheet & theme overrides
│   │   ├── js/                      # Google Maps integration & validation scripts
│   │   └── images/                  # Platform assets and UI icons
│   ├── WEB-INF/
│   │   ├── web.xml                  # Servlet mappings and configuration
│   │   └── lib/                     # JDBC and external library JARs
│   ├── index.jsp                    # Landing home page
│   ├── login.jsp                    # User / Donor / Pharmacy login
│   ├── register.jsp                 # Account creation
│   ├── donor-search.jsp             # Blood donor locator with map
│   ├── emergency-request.jsp        # Raise urgent blood request
│   ├── medicine-finder.jsp          # Pharmacy search & stock viewer
│   └── dashboard.jsp                # Request coordination console
├── database/
│   └── schema.sql                   # MySQL tables schema and initial seed data
├── pom.xml / build.xml              # Project build configuration
└── README.md

---

## 🗄 Database Schema (Sample DDL)
[28-10-2026 10:49] Anil: CREATE DATABASE IF NOT EXISTS medilink_db;
USE medilink_db;

-- Users Table
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(15) NOT NULL,
    role ENUM('PATIENT', 'DONOR', 'VOLUNTEER', 'PHARMACY') NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Donors Table
CREATE TABLE donors (
    donor_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    blood_group ENUM('A+', 'A-', 'B+', 'B-', 'AB+', 'AB-', 'O+', 'O-') NOT NULL,
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    address VARCHAR(255),
    is_available BOOLEAN DEFAULT TRUE,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);

-- Emergency Blood Requests
CREATE TABLE blood_requests (
    request_id INT AUTO_INCREMENT PRIMARY KEY,
    requester_id INT NOT NULL,
    blood_group ENUM('A+', 'A-', 'B+', 'B-', 'AB+', 'AB-', 'O+', 'O-') NOT NULL,
    units_required INT NOT NULL,
    hospital_name VARCHAR(150) NOT NULL,
    hospital_address VARCHAR(255) NOT NULL,
    urgency_level ENUM('LOW', 'MEDIUM', 'CRITICAL') DEFAULT 'CRITICAL',
    status ENUM('OPEN', 'ACCEPTED', 'IN_PROGRESS', 'COMPLETED', 'CANCELLED') DEFAULT 'OPEN',
    assigned_donor_id INT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (requester_id) REFERENCES users(user_id),
    FOREIGN KEY (assigned_donor_id) REFERENCES donors(donor_id)
);

-- Pharmacies & Medicine Inventory
CREATE TABLE pharmacies (
    pharmacy_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(150) NOT NULL,
    phone VARCHAR(15) NOT NULL,
    address VARCHAR(255) NOT NULL,
    latitude DECIMAL(10, 8),
    longitude DECIMAL(11, 8),
    is_24_hours BOOLEAN DEFAULT FALSE
);

CREATE TABLE medicines (
    medicine_id INT AUTO_INCREMENT PRIMARY KEY,
    pharmacy_id INT NOT NULL,
    medicine_name VARCHAR(150) NOT NULL,
    category VARCHAR(100),
    stock_quantity INT DEFAULT 0,
    price DECIMAL(10, 2),
    FOREIGN KEY (pharmacy_id) REFERENCES pharmacies(pharmacy_id) ON DELETE CASCADE
);

---

## ⚙️ Installation & Setup Guide

### 1. Prerequisites
* Java Development Kit (JDK 17 or higher)
* Apache Tomcat 9.0+
* MySQL Server 8.0+
* Eclipse IDE for Enterprise Java and Web Developers or IntelliJ IDEA Ultimate
* Google Maps API Key (with Maps JavaScript API and Geocoding API enabled)

### 2. Database Configuration
1. Open MySQL Workbench or terminal:
   ```bash
   mysql -u root -p < database/schema.sql
   ```
2. Configure credentials in src/main/java/com/medilink/config/DBConnection.java:
   ```java
   private static final String URL = "jdbc:mysql://localhost:3306/medilink_db";
   private static final String USER = "root";
   private static final String PASSWORD = "your_mysql_password";
   ```

### 3. Application Deployment
1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/medilink.git
   ```
2. Import the project into Eclipse as an Existing Dynamic Web Project (or Maven Project).
3. Add the mysql-connector-j-8.x.x.jar into the WEB-INF/lib folder.
4. Add your Google Maps API key into WebContent/assets/js/maps.js:
   ```javascript
   const GOOGLE_MAPS_API_KEY = "YOUR_API_KEY_HERE";
   ```
5. Right-click project → Run As → Run on Server (select Apache Tomcat).
6. Open your browser and navigate to:
   ```text
   http://localhost:8080/MediLink/
   ```

---

## 🏃 Agile Development Plan (Sprints)
[28-10-2026 10:49] Anil: | Sprint | Objective | Deliverables | Status |
| :---: | :--- | :--- | :---: |
| Sprint 1 | System Design & Auth | User sign-up/login, DB schema setup, role routing | ✅ Done |
| Sprint 2 | Blood Donor Module | Donor registration, blood group index, availability toggle | 🔄 In Progress |
| Sprint 3 | Emergency Requests | Urgent request generation, status lifecycles, alert logs | 📅 Scheduled |
| Sprint 4 | Medicine Finder | Pharmacy stock listing, medicine catalog query | 📅 Scheduled |
| Sprint 5 | Google Maps API | Proximity calculations, interactive map views | 📅 Scheduled |
| Sprint 6 | End-to-End Testing | Performance testing, final deployment, project report | 📅 Scheduled |

---

## 📄 License & Academic Integrity

This project is developed as part of the academic coursework for Adaptive Software Engineering (24CI3201). Unauthorized reproduction or commercial distribution without prior approval from the project guide and authors is strictly prohibited.
