# SQL HR Project README

## Project Overview
This project is a simple SQL-based Human Resources (HR) management system. It contains the following main tables:

- **DEPARTMENT**: Stores information about the different departments in the organization.
- **FILES**: Tracks various files related to employees or HR processes.
- **PERSON**: Contains data about the employees (e.g., name, age, contact information).
- **POSITION**: Holds details about the job positions available within the organization.

## Key Features
1. **Department Management**:
   - View department details.
   - Analyze employee distribution across departments.

2. **Employee Information**:
   - Store personal and professional details of employees.
   - Retrieve employee data by department or position.

3. **Position Details**:
   - Define and manage different job positions.
   - Track employees assigned to specific positions.

4. **File Tracking**:
   - Maintain a record of HR-related files (e.g., contracts, performance reviews).

## Database Schema Creation
Below are the SQL scripts to create the database and tables:

1. **Create HR Database:**
   ```sql
   CREATE DATABASE HR;
   ```

2. **Create DEPARTMENT Table:**
   ```sql
   CREATE TABLE DEPARTMENT (
       id INT PRIMARY KEY AUTO_INCREMENT,
       department_name VARCHAR(100) NOT NULL
   );
   ```

3. **Create POSITION Table:**
   ```sql
   CREATE TABLE POSITION (
       id INT PRIMARY KEY AUTO_INCREMENT,
       position_name VARCHAR(100) NOT NULL
   );
   ```

4. **Create FILES Table:**
   ```sql
   CREATE TABLE FILES (
       id INT PRIMARY KEY AUTO_INCREMENT,
       file_data TEXT NOT NULL
   );
   ```

5. **Create PERSON Table:**
   ```sql
   CREATE TABLE PERSON (
       id INT PRIMARY KEY AUTO_INCREMENT,
       code VARCHAR(50) NOT NULL,
       tc_number VARCHAR(11) NOT NULL,
       name VARCHAR(100) NOT NULL,
       surname VARCHAR(100) NOT NULL,
       gender CHAR(1) NOT NULL,
       birth_date DATE NOT NULL,
       in_date DATE,
       out_date DATE,
       department_id INT,
       position_id INT,
       parent_position_id INT,
       manager_id INT,
       tel_nr VARCHAR(15),
       salary DECIMAL(10, 2),
       FOREIGN KEY (department_id) REFERENCES DEPARTMENT(id),
       FOREIGN KEY (position_id) REFERENCES POSITION(id)
   );
   ```

## Example Queries
Here are some example SQL queries to get started:
