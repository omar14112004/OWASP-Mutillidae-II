# OWASP Mutillidae II — Security Analysis & Semgrep Custom Rules

### *Prepared by: Omar Talal Talaat 2305007*

---

## 📌 *Overview*

This repository contains a full end-to-end security analysis on the intentionally vulnerable web application *OWASP Mutillidae II*.
The project includes:

* Vulnerability identification (SQL Injection & XSS)
* Docker setup for running Mutillidae II
* Writing and testing *custom Semgrep rules*
* Running scans and improving rule accuracy
* Git branching workflow
* A full technical security report and demo video

This README serves as the main documentation for the project.

---

## 📌 *1. Environment Setup*

### *Run Mutillidae II using Docker Compose*

bash
git clone https://github.com/webpwnized/mutillidae
cd mutillidae
docker-compose up -d


### *Services Used*

#### *Database (MySQL 5.7)*

* Port: *3306*
* Root password: root
* Database: mutillidae

#### *Web App*

* Image: citizenstig/nowasp
* Runs on: *[http://localhost:8080](http://localhost:8080)*

After setup, you should be able to access the Mutillidae web interface.

---

## 📌 *2. Recon & Vulnerability Identification*

Two major vulnerabilities were found and documented.

---

### 🔥 *Vulnerability #1 — SQL Injection*

*File:* /app/classes/MySQLHandler.php
*Issue:* User input directly concatenated into SQL query.

php
$lQueryString = "SELECT username" .
                "FROM accounts" .
                "WHERE username='" . $pUsername . "'" .
                "AND password='" . $pPassword . "';";


### ❗ Why this is vulnerable?

* No validation
* No escaping
* No prepared statements

### 💥 Exploitation Example


username: admin' --
password: anything


→ Successfully bypasses authentication.

---

### 🔥 *Vulnerability #2 — Reflected/Stored XSS*

*File:* /mutillidae/index.php

php
<input type="text" value="<?php echo $lUserInitials; ?>" />


### ❗ Why this is vulnerable?

Output is *not escaped*, allowing an attacker to insert JavaScript.

### 💥 Exploit Payload


"><script>alert('XSS')</script>


---

## 📌 *3. Semgrep Custom Rules*

Two custom Semgrep rules were written with positive/negative test cases.

---

### ✔ *Rule #1 — SQL Injection Detection*

Detects dangerous concatenation in SQL queries:

* Flags:

  * mysqli_query
  * Concatenated SQL strings
* Does not flag:

  * mysqli_prepare

### ✔ *Rule #2 — XSS Detection*

Detects unsafe printing of user input:

* Flags:

  * echo $_GET[...]
  * echo $_POST[...]
* Ignores:

  * htmlspecialchars() sanitized output

---

## 📌 *4. Scan & Iteration Process*

Semgrep was executed on the Mutillidae codebase:

* *170 files scanned*
* *2 custom rules applied*
* *1 confirmed vulnerability detected*

Rules were improved to reduce false positives and increase accuracy.

---

## 📌 *5. Git Workflow*

A clean Git workflow was followed:

bash
git checkout -b semgrep/mostafa
git add .
git commit -m "Add custom Semgrep rules and documentation"
git push origin semgrep/mostafa


This branch contained:

* Rules (.yaml)
* Tests (/tests)
* Report
* README updates

---

## 📌 *6. Project Structure*


/root
│── README.md
│── Lab4 Secure.pdf
│── semgrep-rules/
│     ├── sql-injection.yaml
│     └── xss-detection.yaml
│── tests/
│     ├── sql_positive.php
│     ├── sql_negative.php
│     ├── xss_positive.php
│     └── xss_negative.php
└── docker-compose.yml


---

## 📌 *7. Demo Video & Report*

* *Technical Report:* included as Lab4 Secure.pdf
* *Demo Video:* (Add link here after uploading)

---

## 📌 *8. References*

* OWASP Mutillidae II: [https://github.com/webpwnized/mutillidae](https://github.com/webpwnized/mutillidae)
* Semgrep Documentation: [https://semgrep.dev/docs/](https://semgrep.dev/docs/)


---

## 📌 *Conclusion*

This project demonstrates:

✔ Running a vulnerable web app
✔ Detecting real vulnerabilities
✔ Writing Semgrep rules like a professional SAST engineer
✔ Using Git best practices
✔ Producing a polished report & documentation

Prepared by *Omar Talal Talaat 2305007*
