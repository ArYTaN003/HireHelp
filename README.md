## Demo 🎥

[![Watch the demo](assets/demo.png)]([https://drive.google.com/file/d/FILE_ID/view?usp=sharing](https://drive.google.com/file/d/1CNY0ew6pxOTO597ssgDxzNhkAhSinZ67/view?usp=sharing))

# Steps to connect the authentication and other relevant tables in Database<br>

## Run the below SQL commands in MySQL Workbench <br>
# Steps to connect the authentication and other relevant tables in Database<br>

## Run the below SQL commands in MySQL Workbench <br>


```sql
CREATE DATABASE hireauth;
USE hireauth;
CREATE TABLE user (
    sid INT PRIMARY KEY AUTO_INCREMENT,
    firstname VARCHAR(40) NOT NULL,
    lastname VARCHAR(20) NOT NULL,
    mobile VARCHAR(15) NOT NULL UNIQUE,
    email VARCHAR(50) NOT NULL UNIQUE,
    userrole ENUM('interviewer', 'candidate') NOT NULL DEFAULT 'candidate',
    password_hash CHAR(60) NOT NULL
);
CREATE TABLE jobs (
    job_ID INT PRIMARY KEY,
    job_title VARCHAR(100),
    location VARCHAR(100),
    descr TEXT,
    requirements TEXT,
    Added_By INT,
    status boolean,
    company_name varchar(200),
    date_of_posting date,
    pay decimal,
    FOREIGN KEY (Added_By) REFERENCES user(sid)
);

CREATE TABLE interviewers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    firstname VARCHAR(50) NOT NULL,
    lastname VARCHAR(50) NOT NULL,
    mobile VARCHAR(20) UNIQUE, -- Ensure mobile numbers are unique
    email VARCHAR(100) UNIQUE, -- Ensure email addresses are unique
    experience_years INT,
    skills JSON, -- JSON column for storing skills
    education JSON, -- JSON column for storing education details
    availability JSON, -- JSON column for storing availability details
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    user_id int,
    FOREIGN KEY (user_id) REFERENCES user(sid)
);

CREATE TABLE application (
    id INT AUTO_INCREMENT UNIQUE KEY,
    jid INT,
    firstname VARCHAR(50) NOT NULL,
    lastname VARCHAR(50) NOT NULL,
    mobile VARCHAR(20) NOT NULL,
    email VARCHAR(100) NOT NULL,
    experience_years INT NOT NULL, -- Store experience in years
    skills JSON, -- JSON column for storing skills
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Timestamp of creation
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES user(sid),
    PRIMARY KEY (jid,user_id)
);

CREATE TABLE scheduled_interview(
    cid INT,
    jid INT,
    job_title VARCHAR(100) NOT NULL,
    candidate_name VARCHAR(100) NOT NULL,
    pmember1 INT NOT NULL,
    pmember2 INT,
    pmember3 INT,
    panel VARCHAR(300) NOT NULL,
    descr TEXT,
    tm VARCHAR(50) NOT NULL,
    dt VARCHAR(50) NOT NULL
);

CREATE TABLE notifications (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    message TEXT NOT NULL,
    is_read BOOLEAN DEFAULT FALSE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES user(sid) ON DELETE CASCADE
);
```

## This creates the database and now update the settings.py <br>

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'hireauth',
        'USER': 'root',                             # <--------------------- Add the user<br>
        'PASSWORD': 'aryan',                     # <--------------------- Add the password<br>
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

## Now Run commands:

```python
py manage.py makemigrations
py manage.py migrate
py manage.py runserver
```
