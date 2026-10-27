# Aarogya_AI/CLIN-BOT

Clin-bot is a RASA-based fullstack application designed for the digitization of sensitive patient data, ranging from visit timings to personal medical history. It enables real-time communication between patients and doctors through an integrated chat feature and provides a foundational chatbot to help new learners get started with medical assistance from day one.

## How to download and use:

### 1] Training the Rasa model and setting up a local server.

To use this instance of Rasa, install rasa and rasa.sdk from the requirements.txt file.
First, make sure all files have been installed properly and set up the structure correctly as follows:

```
main_directory/
│
├── config.yml
├── credentials.yml
├── domains.yml
├── endpoints.yml
│
├── data/
│   ├── nlu.yml
│   ├── rules.yml
│   └── stories.yml
│
├── front-end/
│   ├── app.py
│   ├── static/
│   │   ├── css/
│   │   │   ├── health.css
│   │   │   ├── logged_in_health.css
│   │   │   ├── style_aboutus.css
│   │   │   ├── style_chat.css
│   │   │   ├── style_createacc.css
│   │   │   └── style_loginpage.css
│   │   └── images/
│   │       ├── bg.jpg
│   │       ├── blurred_bg.jpg
│   │       └── healthcare.jpg
│   └── templates/
│       ├── aboutus.html
│       ├── chat_logged_in.html
│       ├── chat.html
│       ├── createacc.html
│       ├── index.html
│       ├── loginpage.html
│       └── logged_in_index.html
│
├── models/
│   └── _some_model.tar
│
└── tests/
    └── test_stories.yml
```

Structure your directory as shown above.

Ensure you have a Python version from 3.8 to 3.11, as Rasa is incompatible with versions newer than 3.11. After ensuring you have a compatible version, preferably Python 3.11, install the following packages from requirements.txt:
```
=============== required packages ===================
rasa
rasa.sdk
pyodbc
bcrypt
flask
Flask-Session
==================++++++++++++========================
```
Once installation is complete, ensure you're in the correct directory with the proper structure. Change your current directory to the main_directory, then run the following commands:
```
===================== cmd prompt ========================
> rasa train
> // store the result in the models subdirectory as shown above.
> rasa run --enable-api --cors="*"  --debug
> // this will run the recently trained rasa model on an output port mostly preferably local-host//server.

===================== cmd prompt ========================
```

### 2] Creating a sql server:

After creating the Rasa server, to access all features from our website, go to the following link:
https://www.microsoft.com/en-in/sql-server/sql-server-downloads/

Scroll down and download SQL Server Express 2022. Proceed with the installation, selecting a basic server configuration when prompted. Upon completion, close out of the setup, and go to your local command prompt to perform SQL commands. Type the following:
```
======================= cmd prompt ============================
sqlcmd -S .\SQLEXPRESS -E
================== end of cmd prompt ===========================



======================= sqlcmd prompt =========================
 CREATE DATABASE model;
 GO
 -- This will create a sql database named model, which can be used as:
 USE model
 GO
 -- Then create the follwing tables which are required to store user login info and chat session infos for dynamic data loading. 
 CREATE TABLE Users (
    UserID INT IDENTITY(1,1) PRIMARY KEY,
    Email NVARCHAR(255) NOT NULL,
    PasswordHash NVARCHAR(255) NOT NULL,
    CreatedAt DATETIME NOT NULL DEFAULT GETDATE(),
    person_name NVARCHAR(100) NOT NULL
 ); 
 -- Creating user data storage, currently prototype form.
 CREATE TABLE ChatSessions (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    UserId INT NOT NULL,
    Session_Id UNIQUEIDENTIFIER NOT NULL DEFAULT NEWID(),
    Session_Title NVARCHAR(255) NULL,
    User_Chat NVARCHAR(MAX) NULL,
    Bot_Chat NVARCHAR(MAX) NULL,
   CreatedAt DATETIME NOT NULL DEFAULT GETDATE(),
    CONSTRAINT FK_Chat_User FOREIGN KEY (UserId) REFERENCES Users(UserID)
 );
 ======================= sqlcmd prompt =========================
```
This should create the required sql tables to work properly with the remaining code.

### 3] Creating a local host-server for testing using app.py
Once you have both the Rasa and SQL servers running, open your code editor and execute the app.py file from this repository. For example, if you are using VS Code, navigate to your working directory and run the Python script with:
python app.py

This should open a site that provides a preview of our clinical chatbot.

<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/BHxhpPbJqY8"
  title="Clin-Bot Demo"
  frameborder="0"
  allowfullscreen>
</iframe>
