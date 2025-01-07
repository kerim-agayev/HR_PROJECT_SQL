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
   WHERE out_date IS NULL;
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

6. **Number of men and women currently working by department (detailed view):**
   ```sql
   SELECT DP.DEPARTMENT, 
   SUM(CASE 
       WHEN PR.GENDER = 'E' THEN 1 ELSE 0
   END) AS MALE_PERSONCOUNT,
   SUM(CASE 
       WHEN PR.GENDER = 'K' THEN 1 ELSE 0
   END) AS FEMALE_PERSON_COUNT 
   FROM PERSON PR
   JOIN DEPARTMENT DP 
   ON PR.DEPARTMENTID = DP.ID
   WHERE PR.OUTDATE IS NULL
   GROUP BY DP.DEPARTMENT
   ORDER BY 1;
   ```

   **Alternative Query:**
   ```sql
   SELECT *,
   (SELECT COUNT(*) FROM PERSON WHERE GENDER = 'K' AND PERSON.DEPARTMENTID = DP.ID AND OUTDATE IS NULL) AS FEMALE_PERSONCOUNT,
   (SELECT COUNT(*) FROM PERSON WHERE GENDER = 'E' AND PERSON.DEPARTMENTID = DP.ID AND OUTDATE IS NULL) AS MALE_PERSONCOUNT 
   FROM DEPARTMENT DP
   ORDER BY 1;
   ```

7. **Salary analysis for the Planning Department's head position:**
   ```sql
   SELECT 'PLANLAMA_SEFI' AS POSITION, MIN(SALARY) MIN_SALARY, MAX(SALARY) MAX_SALARY, ROUND(AVG(SALARY), 0) AVG_SALARY 
   FROM PERSON 
   WHERE POSITIONID = 25;
   ```

   **Alternative Query:**
   ```sql
   SELECT 
   PO.POSITION,
   (SELECT MIN(SALARY) FROM PERSON WHERE POSITIONID = PO.ID) AS MIN_SALARY,
   (SELECT MAX(SALARY) FROM PERSON WHERE POSITIONID = PO.ID) AS MAX_SALARY,
   (SELECT AVG(SALARY) FROM PERSON WHERE POSITIONID = PO.ID) AS AVG_SALARY
   FROM POSITION PO
   WHERE PO.POSITION = 'PLANLAMA ŞEFİ';
   ```

8. **Number of people currently working in each position and their average salaries:**
   ```sql
   SELECT PO.POSITION,
   (SELECT COUNT(*) FROM PERSON WHERE PERSON.POSITIONID = PO.ID AND PERSON.OUTDATE IS NULL) AS PERSON_COUNT,
   (SELECT AVG(SALARY) FROM PERSON  WHERE PERSON.POSITIONID = PO.ID AND PERSON.OUTDATE IS NULL) AS AVG_SALARY
   FROM POSITION PO
   ORDER BY 1;
   ```

   **Alternative Query:**
   ```sql
   SELECT
   PO.POSITION,
   COUNT(PE.ID),
   AVG(PE.SALARY)
   FROM PERSON PE
   JOIN POSITION PO
   ON PO.ID = PE.POSITIONID
   WHERE PE.OUTDATE IS NULL
   GROUP BY  PO.POSITION
   ORDER BY 1;
   ```

9. **Number of personnel hired by year, based on gender:**
   ```sql
   SELECT YEAR(INDATE) YEAR,
   SUM(CASE 
   WHEN GENDER = 'E' THEN 1 ELSE 0
   END) AS MALE_PERSON,
   SUM(CASE 
   WHEN GENDER = 'K' THEN 1 ELSE 0
   END) AS FEMALE_PERSON
   FROM PERSON
   GROUP BY YEAR(INDATE)
   ORDER BY 1;
   ```

   **Alternative Query:**
   ```sql
   SELECT 
   DISTINCT
   YEAR(P.INDATE) YEAR,
   (SELECT COUNT(*) FROM PERSON WHERE GENDER = 'K' AND YEAR(INDATE) = YEAR(P.INDATE)) AS FEMALE_PERSON,
   (SELECT COUNT(*) FROM PERSON WHERE GENDER = 'E' AND YEAR(INDATE) = YEAR(P.INDATE)) AS MALE_PERSON
   FROM PERSON P
   ORDER BY 1;
   ```

10. **Working duration of each personnel in months:**
    ```sql
    SELECT NAME_, INDATE, OUTDATE, 
    CASE 
    WHEN OUTDATE IS NULL THEN DATEDIFF(YY, INDATE, GETDATE()) 
    ELSE DATEDIFF(YY, INDATE, OUTDATE)
    END AS WORKING_TIME
    FROM PERSON;
    ```

    **Alternative Query:**
    ```sql
    SELECT NAME_, INDATE, OUTDATE, 
    DATEDIFF(MONTH, INDATE, GETDATE()) AS WORKING_TIME
    FROM PERSON WHERE OUTDATE IS NULL
    UNION ALL
    SELECT NAME_, INDATE, OUTDATE, 
    DATEDIFF(MONTH, INDATE, OUTDATE) AS WORKING_TIME
    FROM PERSON WHERE OUTDATE IS NOT NULL;
    ```

11. **Initials for Dateboard Printing:**
   ```sql
   SELECT  
   CONCAT(LEFT(NAME_,1) + '.',LEFT(SURNAME,1) + '.' ) AS SHORTNAME,
   COUNT(PERSON.ID) PERSON_COUNT
   FROM PERSON 
   GROUP BY CONCAT(LEFT(NAME_,1) + '.',LEFT(SURNAME,1) + '.' ) 
   ORDER BY COUNT(*) DESC;
   ```

12. **Departments with Average Salary Over 5500:**
   ```sql
   SELECT DEPARTMENT, ROUND(AVG(P.SALARY),0) AVG_SALARY FROM PERSON P 
   JOIN DEPARTMENT D
   ON D.ID = P.DEPARTMENTID
   GROUP BY DEPARTMENT
   HAVING ROUND(AVG(P.SALARY),0) > 5500;
   ```

   **Alternative Query:**
   ```sql
   SELECT
   DEPARTMENT ,
   ( SELECT ROUND(AVG(SALARY),0) FROM PERSON WHERE PERSON.DEPARTMENTID = D.ID) AVGSALARY
   FROM
   DEPARTMENT D
   WHERE ( SELECT ROUND(AVG(SALARY),0) FROM PERSON WHERE PERSON.DEPARTMENTID = D.ID)  > 5500;
   ```

13. **Average Seniority in Departments:**
    ```sql
    SELECT T.DEPARTMENT, AVG(T.WORKINGTIME) AS AVG_WORKINGTIME FROM (
    SELECT  D.DEPARTMENT,
    CASE
    WHEN OUTDATE IS NOT NULL THEN DATEDIFF(MONTH, INDATE, OUTDATE)
    ELSE DATEDIFF(MONTH, INDATE, GETDATE())
    END AS WORKINGTIME
    FROM PERSON P JOIN 
    DEPARTMENT D ON D.ID = P.DEPARTMENTID
    ) AS T 
    GROUP BY T.DEPARTMENT
    ORDER BY 1;
    ```

14. **Personnel and Unit Manager Details:**
    ```sql
    SELECT PE.NAME_ + ' ' + PE.SURNAME AS PERSON, PN.POSITION, PE2.NAME_  + ' ' + PE2.SURNAME AS MANAGER, PN2.POSITION
    FROM PERSON PE 
    JOIN POSITION PN ON PE.POSITIONID = PN.ID
    JOIN PERSON PE2 ON PE.MANAGERID = PE2.ID
    JOIN POSITION PN2 ON PE2.POSITIONID = PN2.ID
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

