# Online CV Builder System

## 📌 Project Overview
The **Online CV Builder System** is a Java-based desktop application developed as part of the **Principles of Database Management** course.  
The system allows users to create, view, update, and manage Curriculum Vitae (CV) information through a graphical user interface connected to a relational database.

The project demonstrates practical usage of **database design**, **SQL**, and **Java Swing** in building a real-world information system.

---

## 🎯 Project Objectives
- Design and implement a database-driven application
- Apply database management concepts in a practical scenario
- Provide a user-friendly interface for CV creation and management
- Perform CRUD operations (Create, Read, Update, Delete) on CV data
- Integrate Java applications with a relational database

---

## 🧩 Project Structure
```
OnlineCVBuilder/
│
├── CV Generator/
│ ├── src/
│ │ ├── cv/
│ │ │ └── generator/
│ │ │ ├── CVGenerator.java # Main application entry point
│ │ │ ├── CVs.java # GUI logic and event handling
│ │ │ ├── CVs.form # NetBeans GUI form
│ │ │ └── db.java # Database connection and queries
│ │ └── Images/ # Icons and UI images
│ │
│ ├── build/ # Compiled classes
│ ├── build.xml # Ant build configuration
│ └── CVs.sql # Database schema and sample data
│
├── .idea/ # IDE configuration
└── README.md # Project documentation
```

---

## 🛠️ Technologies Used
- **Java (JDK 8+)**
- **Java Swing (GUI)**
- **JDBC**
- **SQL (Relational Database)**
- **Apache Ant**
- **NetBeans / IntelliJ IDEA**

---

## 🗄️ Database Design
- The database schema is defined in `CVs.sql`
- Supports storing CV-related information such as:
  - Personal details
  - Education
  - Skills
  - Experience
- Uses SQL queries to perform CRUD operations
- Database connection handled via JDBC in `db.java`

---

## ⚙️ Application Features
- Create a new CV
- View existing CV records
- Update CV information
- Delete CV records
- User-friendly form-based interface
- Database-backed persistent storage

---

## ▶️ How to Run the Project

### 1️⃣ Prerequisites
- Java JDK 8 or higher
- NetBeans / IntelliJ IDEA
- SQL Server or compatible RDBMS
- JDBC driver configured

### 2️⃣ Database Setup
1. Open `CVs.sql`
2. Execute the script in your database management system
3. Update database credentials in `db.java`

### 3️⃣ Run the Application
- Open the project in NetBeans or IntelliJ
- Build the project using Ant
- Run `CVGenerator.java`

---

## 🧠 Learning Outcomes
- Understanding relational database design
- Applying SQL queries in real applications
- Connecting Java applications to databases using JDBC
- Designing GUI-based systems with Java Swing
- Implementing CRUD operations in a database system

---

## 🚀 Future Enhancements
- User authentication and role management
- Export CV to PDF format
- Web-based version of the system
- Improved UI/UX design
- Cloud database integration

---

## 📄 License
This project is developed for **academic purposes**.  
Free to use and modify for learning and educational use.

---

⭐ This project demonstrates practical database management concepts through a real-world application.
