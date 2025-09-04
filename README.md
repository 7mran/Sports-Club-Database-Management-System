# Sports Club Database Management System

A comprehensive relational database system designed to manage member certifications across multiple sports disciplines. This project demonstrates advanced SQL database design principles, normalisation techniques, and complex query implementation for real-world sports club operations.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Database Schema](#database-schema)
- [Features](#features)
- [Installation & Setup](#installation--setup)
- [Usage Examples](#usage-examples)
- [Query Capabilities](#query-capabilities)
- [Technical Highlights](#technical-highlights)
- [Future Enhancements](#future-enhancements)

## 🏆 Project Overview

The Sports Club Database Management System addresses the complex requirements of managing member certifications, instructor assignments, and sports programme administration. The system efficiently handles relationships between members, instructors, sports programs, and assessment records through a well-structured relational design.

### Key Benefits
- **Comprehensive Member Management**: Track member details and certification progress
- **Instructor Assignment System**: Manage instructor workloads and specialisations
- **Flexible Assessment Tracking**: Support for retakes and instructor changes
- **Business Intelligence**: Advanced reporting and analytics capabilities

## 🗄️ Database Schema

### Core Entities

#### Members Table
- **memberID** (Primary Key): Unique member identifier
- **firstName**: Member's first name
- **surname**: Member's surname  
- **phoneNumber**: Contact number (nullable)

#### Instructors Table
- **instructorID** (Primary Key): Unique instructor identifier
- **firstName**: Instructor's first name
- **surname**: Instructor's surname
- **email**: Contact email address

#### Sports Table
- **sportID** (Primary Key): Sport name/identifier
- **durationYears**: Certification validity period
- **fee£**: Annual membership fee

#### Certification Table (Junction Table)
- **memberID** (Foreign Key): References Members
- **instructorID** (Foreign Key): References Instructors  
- **sportID** (Foreign Key): References Sports
- **assessmentMonth**: Month of assessment
- **assessmentYear**: Year of assessment
- **Composite Primary Key**: (memberID, instructorID, sportID)

### Entity Relationship Features
- **Many-to-Many Relationships**: Members can have multiple certifications, instructors can assess multiple sports
- **Referential Integrity**: Foreign key constraints ensure data consistency
- **Flexible Assessment System**: Composite primary key allows multiple assessments per member-sport combination

## ✨ Features

### Data Management
- ✅ Complete CRUD operations (Create, Read, Update, Delete)
- ✅ Proper normalisation with referential integrity
- ✅ Null value handling for optional fields
- ✅ Composite primary key implementation

### Query Capabilities
- 🔍 **Simple Filtering**: Basic member and instructor lookups
- 🔍 **Pattern Matching**: Advanced search with LIKE operators
- 🔍 **Aggregate Functions**: Count, sum, and statistical analysis
- 🔍 **Multi-table Joins**: Complex relationship queries
- 🔍 **Subqueries & EXISTS**: Advanced logical operations
- 🔍 **Sorting & Grouping**: Organised data presentation

### Business Intelligence
- 📊 Member certification tracking
- 📊 Instructor workload analysis
- 📊 Popular sports identification
- 📊 Seasonal assessment patterns
- 📊 Revenue analysis by sport

## 🚀 Installation & Setup

### Prerequisites
- SQL Database System (MySQL, PostgreSQL, SQLite, etc.)
- Database client or command-line interface

### Setup Instructions

1. **Clone the repository**
   ```bash
   git clone [repository-url]
   cd sports-club-database
   ```

2. **Create the database**
   ```sql
   CREATE DATABASE sports_club;
   USE sports_club;
   ```

3. **Run the setup script**
   ```sql
   SOURCE sports_club_setup.sql;
   ```

4. **Verify installation**
   ```sql
   SHOW TABLES;
   SELECT COUNT(*) FROM Members;
   ```

## 💡 Usage Examples

### Basic Member Lookup
```sql
-- Find all members without phone numbers
SELECT firstName, surname 
FROM Members 
WHERE phoneNumber IS NULL;
```

### Complex Analytics
```sql
-- Total certifications per member with LEFT JOIN
SELECT m.memberID, m.firstName, m.surname, 
       COUNT(c.memberID) AS totalCertifications
FROM Members m
LEFT JOIN Certification c ON c.memberID = m.memberID
GROUP BY m.memberID, m.firstName, m.surname;
```

### Advanced Business Queries
```sql
-- Members assessed by specific instructor for specific sport
SELECT m.firstName, m.surname
FROM Members m
WHERE EXISTS(
    SELECT 1 FROM Certification c
    JOIN Instructors i ON c.instructorID = i.instructorID
    WHERE c.memberID = m.memberID
    AND c.sportID = 'Badminton'
    AND c.assessmentYear = 2022
    AND i.firstName = 'Carlo'
    AND i.surname = 'Ancelotti'
);
```

## 🔧 Query Capabilities

### Single Table Operations
- Member and instructor management
- Sports programme configuration
- Assessment date filtering
- Statistical summaries

### Multi-Table Analysis
- Cross-referencing member-instructor relationships
- Sport-specific certification tracking
- Timeline analysis of assessments
- Comprehensive reporting across all entities

### Advanced Features
- **EXISTS Clauses**: Complex logical conditions
- **LEFT JOINs**: Include all records with optional matches
- **Aggregate Functions**: COUNT, SUM with GROUP BY
- **Pattern Matching**: Flexible text searching
- **Date Range Queries**: Temporal analysis capabilities

## 🏗️ Technical Highlights

### Database Design Principles
- **Third Normal Form (3NF)**: Eliminates data redundancy
- **Referential Integrity**: Foreign key constraints prevent orphaned records
- **Composite Keys**: Flexible many-to-many relationship handling
- **Scalable Architecture**: Supports business growth and evolving requirements

### Query Optimisation
- **Indexed Primary Keys**: Fast record retrieval
- **Efficient Joins**: Proper relationship traversal
- **Selective Filtering**: Minimise data processing overhead

### Data Integrity
- **Constraint Enforcement**: Prevents invalid data states
- **NULL Handling**: Graceful management of missing information
- **Transaction Safety**: Consistent data modifications

## 🔮 Future Enhancements

### Planned Features
- **Web Interface**: Browser-based management dashboard
- **Mobile App**: Instructor assessment recording and member progress tracking
- **Automated Reporting**: Scheduled business intelligence reports
- **API Development**: RESTful services for third-party integration

### Advanced Analytics
- **Data Visualisation**: Interactive charts and dashboards
- **Predictive Analysis**: Member retention and performance forecasting
- **Financial Reporting**: Revenue tracking and fee optimisation

### System Improvements
- **User Authentication**: Role-based access control
- **Audit Logging**: Change tracking and compliance
- **Backup & Recovery**: Automated data protection
- **Performance Monitoring**: Query optimisation and scaling

## 📊 Sample Data

The system includes comprehensive test data featuring:
- **10 Members**: Including football personalities with varied contact information
- **10 Instructors**: Premier League managers with email contacts
- **10 Sports**: Ranging from football to power lifting with different durations and fees
- **12 Certifications**: Diverse assessment records across 2022-2023
---
