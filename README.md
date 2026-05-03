Overview
Student CRUD System ek simple aur lightweight web application hai jo PHP aur MySQL (PDO) se bani hai. Isme koi framework use nahi kiya gaya. Yeh system students ke records ko Create, Read, Update aur Delete karne ki sahulat deta hai.

Key highlights: db.php pehli baar run hone par automatically database aur table create kar leta hai — phpMyAdmin ya SQL import ki zarurat nahi.

Technology Stack
•	PHP 7.4+ — Server-side scripting
•	MySQL 5.7+ / MariaDB 10.3+ — Database
•	PDO (PHP Data Objects) — Secure database access with prepared statements
•	HTML5 & CSS3 — Frontend markup and styling
•	No framework, no Composer, no npm — pure PHP

Requirements
•	PHP 7.4 or higher
•	MySQL 5.7+ or MariaDB 10.3+
•	Apache / Nginx web server
•	XAMPP, WAMP, LAMP, or any PHP-MySQL environment
•	Web browser (Chrome, Firefox, Edge, etc.)

Project File Structure
student-crud/
  ├── db.php              ← Database connection & auto-setup
  ├── index.php           ← Student list (Read)
  ├── create.php          ← Add student (Create)
  ├── edit.php            ← Update student (Update)
  ├── delete.php          ← Remove student (Delete)
  ├── schema.sql          ← Manual SQL import (optional)
  └── assets/
      └── style.css       ← All styling

File	Location	Description
db.php	/ (root)	PDO database connection — auto-creates DB & table on first run
index.php	/ (root)	Student list with Edit / Delete actions and status alerts
create.php	/ (root)	Add new student form with server-side validation
edit.php	/ (root)	Edit existing student record (pre-filled form)
delete.php	/ (root)	Deletes a student by ID then redirects to index.php
schema.sql	/ (root)	SQL script to manually create database and students table
assets/style.css	assets/	All CSS — layout, cards, table, buttons, form, responsive

Setup & Installation
Option 1 — XAMPP (Recommended for Beginners)
1.	XAMPP download aur install karein: https://www.apachefriends.org
2.	XAMPP Control Panel se Apache aur MySQL start karein
3.	Project folder C:/xampp/htdocs/student-crud/ mein copy karein
4.	Browser mein kholain:  http://localhost/student-crud/
5.	db.php automatically database aur table create kar lega

Option 2 — PHP Built-in Server
cd /path/to/student-crud
php -S localhost:8000
Phir browser mein kholain: http://localhost:8000/
Note: Built-in server ke saath MySQL alag se run hona chahiye.

Option 3 — Manual SQL Import
Agar db.php auto-setup na kare to schema.sql phpMyAdmin mein import karein:
6.	phpMyAdmin kholain: http://localhost/phpmyadmin
7.	'New' database banao: crud_system
8.	'Import' tab par jayen aur schema.sql select karein
9.	'Go' click karein — table ready ho jayegi

Database Structure
Database name: crud_system     Table name: students

Column	Type	Nullable	Notes
id	INT UNSIGNED	NO	Auto-increment primary key
first_name	VARCHAR(100)	NO	Student first name
last_name	VARCHAR(100)	NO	Student last name
reg_no	VARCHAR(50)	NO	Registration number — UNIQUE constraint
email	VARCHAR(150)	NO	Email address — UNIQUE constraint
dob	DATE	YES	Date of birth (optional)
course	VARCHAR(150)	YES	Enrolled course (optional)
created_at	DATETIME	YES	Auto-set to NOW() on insert

Both reg_no and email columns have UNIQUE constraints — duplicate values will cause a database error.

Pages & Routes

URL	Method	What it does
index.php	GET	Lists all students ordered by newest first
create.php	GET	Shows empty Add Student form
create.php	POST	Validates & inserts new student, redirects on success
edit.php?id=N	GET	Shows Edit form pre-filled with student data
edit.php?id=N	POST	Validates & updates student record, redirects on success
delete.php?id=N	GET	Deletes student by ID and redirects with ?status=deleted

How It Works — Flow
db.php — Auto Database Setup
•	Pehle existing database se connect karne ki koshish karta hai
•	Agar database nahi mili to khud CREATE DATABASE run karta hai
•	CREATE TABLE IF NOT EXISTS se table ensure karta hai
•	static $pdo se single connection per request maintain karta hai

Validation (create.php & edit.php)
•	First name aur last name required hain
•	Registration number required hai
•	Email PHP ke filter_var(FILTER_VALIDATE_EMAIL) se validate hoti hai
•	Errors $errors[] array mein collect hote hain aur form ke upar display hote hain
•	Successful submission ke baad header('Location: index.php?status=...') redirect karta hai

Status Alerts (index.php)
•	?status=created  — 'Student created successfully' alert
•	?status=updated  — 'Student updated successfully' alert
•	?status=deleted  — 'Student deleted successfully' alert

Security Features
•	PDO Prepared Statements — SQL injection se protection, sab queries ? placeholders use karti hain
•	htmlspecialchars() — XSS attacks se bachne ke liye sab output escape hoti hai
•	(int) casting — delete.php aur edit.php mein ID integer mein cast hoti hai
•	Input trimming — trim() se leading/trailing whitespace hata di jati hai
•	FILTER_VALIDATE_EMAIL — email format server-side validate hoti hai

Styling (assets/style.css)
•	CSS Variables — accent, danger, success, bg, text, border colors
•	Gradient backgrounds — header, buttons, table rows
•	Sticky header — position: sticky; top: 0 with z-index: 100
•	Card component — white card with box-shadow and hover lift effect
•	Responsive — @media (max-width: 600px) for mobile screens
•	Hover transitions — buttons lift on hover (translateY -2px)
•	Error box — red-tinted background with left-border for validation errors

Customization
DB Credentials Change Karna
db.php ke upar ye constants edit karein:
define('DB_HOST', 'localhost');   // MySQL server
define('DB_NAME', 'crud_system'); // Database name
define('DB_USER', 'root');        // MySQL username
define('DB_PASS', '');            // MySQL password

Naya Field Add Karna
10.	schema.sql mein ALTER TABLE se column add karein
11.	create.php aur edit.php forms mein naya input field add karein
12.	db.php ke CREATE TABLE statement mein bhi column add karein
13.	index.php table mein naya <th> aur <td> add karein

Troubleshooting
'Database connection failed' Error
•	XAMPP mein MySQL service start hai? Control Panel check karein
•	db.php mein DB_USER aur DB_PASS sahi hain?
•	MySQL root user ka password set hai? DB_PASS mein dalen

'Duplicate entry' Error
•	Same email ya reg_no se record pehle se exist karta hai
•	Dono columns par UNIQUE constraint hai
•	Alag email / reg_no use karein ya pehle existing record edit karein
