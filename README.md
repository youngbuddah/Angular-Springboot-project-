# Installing MariaDB, Setting Password, and Importing Database on Ubuntu Linux

This guide will walk you through the process of **installing MariaDB**, setting a password, and importing a database from a SQL file on **Ubuntu Linux**. MariaDB is a popular open-source relational database management system commonly used in web development environments.

---

## 🚀 Step 1: Update Packages and Install MariaDB

Before installing new software, update your package lists:

```bash
sudo apt update
sudo apt install mariadb-server
```

Start and enable MariaDB service:

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

---

## 🚀 Step 2: Secure MariaDB Installation

Run the security script to set root password and remove insecure defaults:

```bash
sudo mysql_secure_installation
```

---

## 🚀 Step 3: Login to MariaDB

Login as root:

```bash
sudo mysql -u root -p
```

---

## 🚀 Step 4: Create Database and User

Inside the MariaDB prompt, create your database and user:

```sql
CREATE DATABASE springbackend;

GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'your_password';

FLUSH PRIVILEGES;
```

Replace `username` and `your_password` with your preferred credentials.

---

## 🚀 Step 5: Import SQL File

Exit the MariaDB prompt and import your database from a `.sql` file:

```bash
sudo mysql -u username -p springbackend < springbackend.sql
```

---

## 🚀 Step 6: Verify Database

Login again as root or the new user:

```bash
sudo mysql -u root -p
```

Check your databases and tables:

```sql
SHOW DATABASES;
USE springbackend;
SHOW TABLES;
SELECT * FROM tbl_workers;
```

---

✍️ **Author:** Abhay Bendekar  
🔗 [LinkedIn](https://www.linkedin.com/in/abhay-bendekar-75474b372/)
