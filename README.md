# HabitHive

A web-based habit tracking application to help users build consistency, stay motivated, and visualize their progress.

Features:

  User Authentication – Register and log in securely.

  Dashboard – Overview of all habits and daily progress.

  Habit Management – Add, edit, and track habits with ease.

  Statistics & Insights – View progress charts and streaks.

  Responsive Design – Works on both desktop and mobile.

Project Structure:

Habit Tracker/
│── app.py              
│── forms.py            
│── models.py           
│── static/
│   ├── style.css       
│   ├── script.js       
│   └── images/         
│── templates/          
│   ├── login.html
│   ├── register.html
│   ├── dashboard.html
│   ├── habit.html
│   ├── hive_stats.html
│   └── blog_detail.html

Installation:

Create a virtual environment:

  python -m venv venv
  source venv/bin/activate   # Linux/Mac
  venv\Scripts\activate      # Windows


Install dependencies:

  pip install -r requirements.txt


Run the app:

  python app.py

Open in browser

Tech Stack:

  Backend: Flask, SQLAlchemy - SQL Server
  
  Frontend: HTML, CSS, JavaScript
  
  Database: SQL Server

Screenshots:

  Dashboard
<img width="1898" height="845" alt="image" src="https://github.com/user-attachments/assets/158e0794-7244-47f9-b8a1-f51b809ccaae" />

  Login
<img width="1916" height="880" alt="image" src="https://github.com/user-attachments/assets/ef0ab300-881b-47c0-9b32-c90cabfd76ca" />

  Habits
<img width="1891" height="875" alt="image" src="https://github.com/user-attachments/assets/22a7c755-3569-4a80-9ad8-c76ac3c8c7a9" />

  Hive Stats
<img width="1896" height="881" alt="image" src="https://github.com/user-attachments/assets/93bfc5a4-a02c-4701-bb6e-660bfec07717" />
