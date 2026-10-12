
# Login & To-Do Application

A secure, responsive task-management web application built with Flask and Microsoft SQL Server. Users can create an account, sign in, and manage a private to-do list through a clean dark-mode interface.

## Author

**Kareem Mohamed Emara** · Student ID: **2304032**

## Overview

The application demonstrates the integration of a web interface, server-side logic, and a relational database. It supports user authentication and personal task management while applying database constraints, parameterized SQL queries, session-based access control, and password hashing.

## Technology

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Database | Microsoft SQL Server |
| Database connectivity | `pyodbc`, ODBC Driver 17 |
| Frontend | HTML, CSS, JavaScript, Jinja |
| Authentication | Flask sessions, bcrypt |
| Configuration | `python-dotenv` |

## Features

- **Account management:** Registration, login, logout, duplicate-email handling, and clear authentication errors.
- **Password security:** Passwords are stored as bcrypt hashes rather than plain text.
- **Private task lists:** Each signed-in user sees their own tasks, ordered newest first.
- **Task management:** Add tasks, edit titles, mark tasks as done or open, and delete tasks with confirmation.
- **Input validation:** Browser-side and server-side validation for required fields and task-title length.
- **Responsive interface:** Consistent dark theme, validation feedback, completed-task styling, and an empty-list message.
- **Bonus features:** Open-task counter and All / Open / Done filters.

## Project Structure

```text
Assignment_Lab2/
├── database/
│   └── schema.sql
├── screenshots/
├── static/
│   └── style.css
├── templates/
│   ├── login.html
│   ├── register.html
│   └── todos.html
├── .env.example
├── .gitignore
├── app.py
├── db.py
└── README.md
```

## Setup

### Prerequisites

- Python 3
- Microsoft SQL Server
- ODBC Driver 17 for SQL Server
- SQL Server Management Studio (SSMS), to execute the schema

### Run locally on Windows

1. **Create a virtual environment** in the project directory:

   ```powershell
   python -m venv .venv
   .\.venv\Scripts\activate
   ```

2. **Install dependencies:**

   ```powershell
   python -m pip install Flask pyodbc python-dotenv bcrypt
   ```

3. **Create the database.** Open `database/schema.sql` in SSMS and execute the final schema script on your SQL Server instance. It should create the `registration` database and the `users` and `todos` tables.

4. **Configure the environment.** Copy `.env.example` to `.env` and set a new, private `SECRET_KEY`:

   ```powershell
   Copy-Item .env.example .env
   ```

5. **Verify the connection settings** in `db.py`. The current setup uses ODBC Driver 17, `localhost`, the `registration` database, and Windows trusted authentication. Adjust these settings if your SQL Server configuration differs.

6. **Start the application:**

   ```powershell
   python app.py
   ```

7. Open the local URL printed by Flask, usually `http://127.0.0.1:5000`.

Keep `.env` private; commit `.env.example` with placeholder values only. Never upload `.env`, `.venv`, or secret keys to the repository.

## Screenshots

The following screenshots document the required application states. Save the images in the `screenshots/` directory using the filenames shown below.

### Registration with validation errors

![Registration validation](screenshots/register-validation.png)

### Login page

![Login page](screenshots/login.png)

### To-do list with a completed task

![To-do list with one completed task](screenshots/todos-one-done.png)

### Responsive layout at phone width

![Mobile layout](screenshots/mobile-view.png)

## Database Design and Security

The `users` table uses a primary key and a unique email constraint. The `todos` table has its own primary key and a foreign key referencing `users.user_id`. Required columns use `NOT NULL`.

Database values are passed as query parameters rather than concatenated into SQL strings. The application takes the current user's ID from the session and uses it when retrieving or changing tasks, preventing users from managing another user's items through a task ID alone.

## Required Questions

**1. Why do `UPDATE` and `DELETE` contain `AND user_id = ?`, and what changes if it is removed?**

The condition matches the task ID and the ID of the signed-in user. It ensures a user can update or delete only their own tasks. Without it, a request containing another user's task ID could modify or delete that task.

**2. What happens if a to-do is inserted for a `user_id` that does not exist, and which constraint is this?**

The database rejects the insert because the `user_id` does not reference an existing row in `users`. This is the **foreign key constraint**, which enforces referential integrity.

**3. Why store a hash instead of the password?**

The application stores a bcrypt hash rather than the original password. This reduces the risk of exposing users' actual passwords if the database is compromised and allows the submitted password to be verified without storing it in plain text.

**4. If `name` is `NOT NULL`, why is a separate empty-name check necessary?**

`NOT NULL` prevents SQL `NULL`, but an empty string (`''`) is still a value. Application-level validation is therefore needed to reject empty or whitespace-only names.

---

