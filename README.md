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

1. **List all employees in a specific department:**

   ```sql
   SELECT *
   FROM PERSON
   WHERE department_id = 1;
   ```

2. **Get details of employees holding a specific position:**

   ```sql
   SELECT p.name, po.position_name
   FROM PERSON p
   JOIN POSITION po ON p.position_id = po.id
   WHERE po.position_name = 'Manager';
   ```

3. **Count the number of employees in each department:**

   ```sql
   SELECT d.department_name AS department_name, COUNT(p.id) AS employee_count
   FROM DEPARTMENT d
   LEFT JOIN PERSON p ON d.id = p.department_id
   GROUP BY d.department_name;
   ```

4. **List of workers still working at the company:**

   ```sql
   SELECT *
   FROM PERSON
   WHERE out_date IS NOT NULL;
   ```

5. **Number of women and men currently working in the company by department:**

   ```sql
   SELECT DP.DEPARTMENT, PR.GENDER, COUNT(PR.ID) AS PERSON_COUNT 
   FROM PERSON PR
   JOIN DEPARTMENT DP 
   ON PR.DEPARTMENTID = DP.ID
   WHERE PR.OUTDATE IS NULL
   GROUP BY DP.DEPARTMENT, PR.GENDER
   ORDER BY 1, 2;
   ```

   **Alternative Query:**

   ```sql
   SELECT 
   DP.DEPARTMENT, 
   CASE
       WHEN PR.GENDER = 'E' THEN 'KİŞİ'
       WHEN PR.GENDER = 'K' THEN 'QADIN'
   END AS GENDER,
   COUNT(PR.ID) AS PERSON_COUNT 
   FROM PERSON PR
   JOIN DEPARTMENT DP 
   ON PR.DEPARTMENTID = DP.ID
   WHERE PR.OUTDATE IS NULL
   GROUP BY DP.DEPARTMENT, GENDER
   ORDER BY 1, 2;
   ```

## Setup

1. Create the database schema using your preferred SQL tool.
2. Populate the tables with initial data.
3. Use the provided queries or write your own to interact with the data.

## Future Enhancements

- Add tables for payroll management.
- Implement performance tracking.
- Integrate with external tools for analytics and reporting.

## Contact

For questions or contributions, please contact [Your Name] at [Your Email Address].

