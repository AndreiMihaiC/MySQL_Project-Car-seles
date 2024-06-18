<h1>Database Project for "Dacia car dealership"</h1>

The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

Application under test: Dacia car dealership

Tools used: MySQL Workbench

Database description: The project manages the structure and interactions between employees, branches, customers and suppliers. It uses DDL to define the structure of tables and the relationships between them, DML to manipulate data (insert, update, delete), DQL to query the database and extract relevant information, as well as joins to combine data from multiple tables for complex reports."

<ol>
<li>Database Schema </li>
<br>
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

The tables are connected in the following way:

![ERR Diagram](https://github.com/AndreiMihaiC/MySQL_Project-Car-seles/assets/120325527/e56090a3-3b40-4828-9942-884bc919f381)

<li>Database Queries</li><br>

<ol type="a">
  <li>DDL (Data Definition Language)</li>

  The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

```
CREATE DATABASE Dacia;

CREATE TABLE employees(
employee_id INT PRIMARY KEY AUTO_INCREMENT,
first_name VARCHAR(20),
last_name VARCHAR(20),
dateOfBirth DATE,
sex VARCHAR(1),
manager_id INT,
branch_office_id INT
);

CREATE TABLE branch_office(
branch_office_id INT PRIMARY KEY,
branch_office_name VARCHAR(40),
manager_id INT,
FOREIGN KEY(manager_id) REFERENCES employees(employee_id) ON DELETE SET NULL
);

CREATE TABLE client (
  client_id INT PRIMARY KEY,
  client_name VARCHAR(40),
  client_details VARCHAR(40),
  branch_office_id INT,
  FOREIGN KEY(branch_office_id) REFERENCES branch_office(branch_office_id) ON DELETE SET NULL
);

CREATE TABLE works_with (
  employee_id INT,
  client_id INT,
  car_models_sold VARCHAR(40),
  PRIMARY KEY(employee_id, client_id),
  FOREIGN KEY(employee_id) REFERENCES employees(employee_id) ON DELETE CASCADE,
  FOREIGN KEY(client_id) REFERENCES client(client_id) ON DELETE CASCADE
);
  
CREATE TABLE branch_supplier (
  branch_office_id INT,
  supplier_name VARCHAR(40),
  supply_type VARCHAR(40),
  PRIMARY KEY(branch_office_id, supplier_name),
  FOREIGN KEY(branch_office_id) REFERENCES branch_office(branch_office_id) ON DELETE CASCADE
);
```


  After the database and the tables have been created, a few ALTER instructions were written in order to update the structure of the database, as described below:
```
ALTER TABLE employees
ADD FOREIGN KEY(branch_office_id)
REFERENCES branch_office(branch_office_id)
ON DELETE SET NULL;

ALTER TABLE employees
ADD FOREIGN KEY(manager_id)
REFERENCES employees(employee_id)
ON DELETE SET NULL;

ALTER TABLE employees ADD COLUMN phone INT;

ALTER TABLE employees DROP COLUMN phone;

ALTER TABLE employees MODIFY first_name CHAR(40) NOT NULL;

ALTER TABLE employees MODIFY last_name CHAR(25) NOT NULL;

ALTER TABLE employees CHANGE last_name employeesLastName CHAR(25) NOT NULL;

ALTER TABLE employees RENAME TO team_members;

ALTER TABLE employees CHANGE employeesLastName last_name VARCHAR(25);

ALTER TABLE employees ADD COLUMN phone DATE;

ALTER TABLE employees DROP COLUMN phone;

ALTER TABLE employees ADD COLUMN phone VARCHAR(40);

ALTER TABLE employees ADD COLUMN email VARCHAR(50);

ALTER TABLE employees ADD COLUMN salary INT;
```

  <li>DML (Data Manipulation Language)</li>

  In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. 
  In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase. 

  Below you can find all the insert instructions that were created in the scope of this project:

```
INSERT INTO employees(employee_id, first_name, last_name, dateOfBirth, sex, manager_id, branch_office_id) VALUES (100, 'Andrei','Mihai','1993-12-19','M', NULL, NULL);
               
INSERT INTO branch_office VALUES (1, 'Dacia North ', 100);

INSERT INTO employees(employee_id, first_name, last_name, dateOfBirth, sex, manager_id, branch_office_id) VALUES (101, 'Alexandra','Popescu','1996-10-18','F', 100, 1);

INSERT INTO employees(employee_id, first_name, last_name, dateOfBirth, sex, manager_id, branch_office_id) VALUES (102, 'Tudor','Andronache','2000-05-03','M',100, NULL);

INSERT INTO branch_office VALUES (2, 'Dacia East ', 102);

INSERT INTO employees(employee_id, first_name, last_name, dateOfBirth, sex, manager_id, branch_office_id) VALUES (103, 'Bianca','Laurentiu','1989-01-21','F',102, 2),
		(104, 'Paul','Oltean','2004-09-09','M', 102, 2),
		(105, 'Marcel','Mihalcea','1998-02-12','M', 102, 2);
        
        
INSERT INTO employees(employee_id, first_name, last_name, dateOfBirth, sex, manager_id,branch_office_id) VALUES (106, 'Laura','Scott','2001-11-21','F',100, NULL);

INSERT INTO branch_office VALUES (3, 'Dacia South ', 106);

INSERT INTO employees(employee_id, first_name, last_name, dateOfBirth, sex, manager_id, branch_office_id) VALUES (107, 'Ana','Ignat','1986-09-15','F',106, 3),
		(108, 'Nicolate','Mandiv','2000-08-09','F', 106, 3),
		(109, 'Marius','Tudose','1999-10-17','M', 106, 3);

INSERT INTO branch_supplier VALUES(2, 'Service Dacia', 'Auto Service');
INSERT INTO branch_supplier VALUES(1, 'Wash and Go', 'Car wash');
INSERT INTO branch_supplier VALUES(3, 'Registration', 'Car registration documents');
INSERT INTO branch_supplier VALUES(2, 'Smile', 'Car towing');
INSERT INTO branch_supplier VALUES(1, 'Office tools', 'Office supplies');
INSERT INTO branch_supplier VALUES(3, 'IT Dacia', 'IT support');
INSERT INTO branch_supplier VALUES(3, 'Sales Office AM', 'Car sales');

INSERT INTO client VALUES(400, 'Antom Paul',"Individual person", 1);
INSERT INTO client VALUES(401, 'Petrea Monica',"Individual person", 2);
INSERT INTO client VALUES(402, 'Ophar Marta',"Legal entity", 3);
INSERT INTO client VALUES(403, 'Mario Parsa',"Individual person", 3);
INSERT INTO client VALUES(404, 'Emily Johnson',"Legal entity", 2);
INSERT INTO client VALUES(405, 'Sandra Mircea',"Individual person", 1);
INSERT INTO client VALUES(406, 'Michael Davis',"Individual person", 2);
INSERT INTO client VALUES(407, 'Paul Maria',"Legal entity", 1);
INSERT INTO client VALUES(408, 'Sarah Smith',"Individual person", 3);
INSERT INTO client VALUES(409, 'David Wilson',"Individual person", 2);
INSERT INTO client VALUES(410, 'Oltean Mircea',"Legal entity", 3);
INSERT INTO client VALUES(411, 'Jennifer Brown',"Individual person", 1);
INSERT INTO client VALUES(412, 'Christopher Taylor',"Individual person", 3);
INSERT INTO client VALUES(413, 'Catrin Martha',"Legal entity", 2);

INSERT INTO works_with VALUES(100, 400, "Dacia Logan MCV");
INSERT INTO works_with VALUES(100, 401, "Dacia Logan");
INSERT INTO works_with VALUES(101, 402,"Dacia Sandero");
INSERT INTO works_with VALUES(102, 403, "Dacia Logan");
INSERT INTO works_with VALUES(102, 404, "Dacia Sandero Stepway");
INSERT INTO works_with VALUES(103, 405, "Dacia Duster");
INSERT INTO works_with VALUES(103, 406, "Dacia Spring");
INSERT INTO works_with VALUES(104, 407, "Dacia Jogger");
INSERT INTO works_with VALUES(105, 408,"Dacia Logan");
INSERT INTO works_with VALUES(106, 409, "Dacia Duster");
INSERT INTO works_with VALUES(106, 410, "Dacia Jogger");
INSERT INTO works_with VALUES(107, 411,"Dacia Dokker");
INSERT INTO works_with VALUES(108, 412, "Dacia Logan MCV");
INSERT INTO works_with VALUES(108, 413, "Dacia Duster");
```

  After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:
```
UPDATE employees
SET branch_office_id = 1
WHERE employee_id = 100;

UPDATE employees
SET branch_office_id = 1
WHERE employee_id = 100;

UPDATE employees
SET branch_office_id = 2
WHERE employee_id = 102;

UPDATE employees
SET branch_office_id = 3
WHERE employee_id = 106;

UPDATE employees SET first_name = 'Popescu';

UPDATE employees SET email ='mihai200@yahoo.com' WHERE employee_id =100;
UPDATE employees SET dateOfBirth = '1895-05-06' WHERE last_name = 'Smith';
UPDATE employees SET first_name = 'Andreas' WHERE last_name = 'Oltean';

UPDATE employees SET salary = 3800 WHERE last_name = 'Mihai';
UPDATE employees SET salary = 7200 WHERE last_name = 'Laurentiu';
UPDATE employees SET salary = 2800 WHERE last_name = 'Mandiv';
UPDATE employees SET salary = 5650 WHERE last_name = 'Smith';
```

  <li>DQL (Data Query Language)</li>

After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean: 
```
DELETE FROM employees;

TRUNCATE TABLE employees;
```

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:
```
SELECT * FROM employees;

SELECT * FROM employees;								
SELECT first_name, last_name FROM employees;				
SELECT employee_id, first_name FROM employees;

SELECT * FROM employees WHERE manager_id = 102;
SELECT * FROM employees WHERE dateOfBirth > '1999-12-31';
SELECT * FROM employees WHERE employee_id = 111;
SELECT * FROM employees WHERE manager_id > 102;

SELECT * FROM employees WHERE last_name IN ('Mihai','Ignat');		
SELECT * FROM employees WHERE last_name NOT IN ('Mihai','Ignat');

SELECT * FROM employees WHERE phone IS NULL;				
SELECT * FROM employees WHERE phone IS NOT NULL;	

SELECT * FROM employees WHERE last_name != 'Mandiv';	

SELECT * FROM employees WHERE employee_id BETWEEN 106 AND 111;

SELECT * FROM employees WHERE last_name LIKE 't%';			
SELECT * FROM employees WHERE last_name LIKE '%on';		
SELECT * FROM employees WHERE last_name LIKE '%n%';
SELECT * FROM employees WHERE dateOfBirth LIKE '1986%';
SELECT * FROM employees WHERE dateOfBirth LIKE '198%';		
SELECT * FROM employees WHERE dateOfBirth LIKE '19%';		
SELECT * FROM employees WHERE dateOfBirth LIKE '%03';		
SELECT * FROM employees WHERE dateOfBirth LIKE '%10%';		
SELECT * FROM employees WHERE dateOfBirth LIKE '%-07-%';

SELECT * FROM employees WHERE last_name LIKE '%on' AND dateOfBirth LIKE '198%';	
SELECT * FROM employees WHERE last_name LIKE '%on' OR dateOfBirth LIKE '197%';
SELECT * FROM employees WHERE last_name LIKE '%on' OR dateOfBirth LIKE '197%' AND employee_id = '110';
SELECT * FROM employees WHERE (last_name LIKE '%on' OR dateOfBirth LIKE '198%') AND employee_id = '103';

SELECT SUM(salary) FROM employees;			
SELECT AVG(salary) FROM employees;			
SELECT MIN(salary) FROM employees;			
SELECT MAX(salary) FROM employees;			
SELECT COUNT(*) FROM employees;

SELECT * FROM employees CROSS JOIN branch_office;

SELECT employees.employee_id, employees.first_name, employees.last_name, branch_office.branch_office_id, branch_office.branch_office_name FROM employees JOIN branch_office ON employees.employee_id = branch_office.manager_id;

SELECT employees.employee_id, employees.first_name, employees.last_name, branch_office.branch_office_id, branch_office.branch_office_name FROM employees LEFT JOIN branch_office ON employees.employee_id = branch_office.manager_id;

SELECT employees.employee_id, employees.first_name, employees.last_name, branch_office.branch_office_id, branch_office.branch_office_name FROM employees RIGHT JOIN branch_office ON employees.employee_id = branch_office.manager_id;

SELECT * FROM employees ORDER BY dateOfBirth;		
SELECT * FROM employees ORDER BY last_name;		
SELECT * FROM employees ORDER BY dateOfBirth ASC;
SELECT * FROM employees ORDER BY dateOfBirth DESC;
SELECT * FROM employees ORDER BY employee_id ASC LIMIT 1;
SELECT * FROM employees ORDER BY employee_id ASC LIMIT 3;

SELECT e.employee_id, e.first_name, e.last_name, COUNT(b.branch_office_id) 
FROM employees e 
INNER JOIN branch_office b ON e.employee_id = b.manager_id 
GROUP BY e.employee_id, e.first_name, e.last_name;

SELECT e.employee_id, e.first_name, e.last_name, COUNT(b.branch_office_id) 
FROM employees e INNER JOIN branch_office b ON e.employee_id = b.manager_id 
GROUP BY e.employee_id, e.first_name, e.last_name HAVING count(b.branch_office_id)>0;

```


**Inserati aici toate instructiunile de SELECT pe care le-ati scris folosind filtrarile necesare astfel incat sa extrageti doar datele de care aveti nevoie**
**Incercati sa acoperiti urmatoarele:**<br>
**- where**<br>
**- AND**<br>
**- OR**<br>
**- NOT**<br>
**- like**<br>
**- inner join**<br>
**- left join**<br>
**- OPTIONAL: right join**<br>
**- OPTIONAL: cross join**<br>
**- functii agregate**<br>
**- group by**<br>
**- having**<br>
**- OPTIONAL DAR RECOMANDAT: Subqueries - nu au fost in scopul cursului. Puteti sa consultati tutorialul [asta](https://www.techonthenet.com/mysql/subqueries.php) si daca nu intelegeti ceva contactati fie trainerul, fie coordonatorul de grupa**<br>

</ol>

<li>Conclusions</li>

**Inserati aici o concluzie cu privire la ceea ce ati lucrat, gen lucrurile pe care le-ati invatat, lessons learned, un rezumat asupra a ceea ce ati facut si orice alta informatie care vi se pare relevanta pentru o concluzie finala asupra a ceea ce ati lucrat**

</ol>
