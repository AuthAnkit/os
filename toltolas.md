# OSTF Practicals 12–16 — Exam Steps

## Practical 12 — Abstract Class and Inheritance (Shape, Triangle, Rectangle)

1. Import `ABC` and `abstractmethod` from `abc`.
2. Create a base class `Shape(ABC)`.
3. In `Shape`, define a constructor or `get_data()` method to store two dimensions, such as `x` and `y`.
4. Define an abstract method `display_area()` using `@abstractmethod`.
5. Create `Rectangle(Shape)` and implement `display_area()` as `x * y`.
6. Create `Triangle(Shape)` and implement `display_area()` as `(x * y) / 2`.
7. Take dimensions from the user.
8. Create the required object and call `display_area()`.
9. Viva: inheritance lets child classes reuse or extend a parent class; an abstract class cannot be instantiated while it has unimplemented abstract methods.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    def __init__(self, x, y):
        self.x = x
        self.y = y

    @abstractmethod
    def display_area(self):
        pass

class Rectangle(Shape):
    def display_area(self):
        print("Rectangle area:", self.x * self.y)

class Triangle(Shape):
    def display_area(self):
        print("Triangle area:", (self.x * self.y) / 2)

x = float(input("Enter first dimension: "))
y = float(input("Enter second dimension: "))
choice = input("Enter rectangle or triangle: ").lower()

if choice == "rectangle":
    shape = Rectangle(x, y)
elif choice == "triangle":
    shape = Triangle(x, y)
else:
    print("Invalid choice")
    raise SystemExit

shape.display_area()
```

## Practical 13 — Interaction with SQL (Books database)

1. Check which database your lab uses (for example, SQLite or MySQL) and use the required connector.
2. Create or connect to the database.
3. Create table `books_master` if it does not already exist, with columns `book_id`, `book_name`, and `author_name`.
4. Take the three values from the user.
5. Use a parameterized `INSERT` query to insert the book.
6. Commit the transaction.
7. Run `SELECT * FROM books_master`.
8. Fetch and display every row.
9. Close the cursor and connection.
10. Viva: `INSERT` adds data, `SELECT` retrieves data, `commit()` saves changes, and parameterized queries help prevent SQL injection.

SQLite example (works without installing a separate database server):

```python
import sqlite3

connection = sqlite3.connect("books.db")
cursor = connection.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS books_master (
    book_id INTEGER PRIMARY KEY,
    book_name TEXT,
    author_name TEXT
)
""")

book_id = int(input("Enter book ID: "))
book_name = input("Enter book name: ")
author_name = input("Enter author name: ")

cursor.execute(
    "INSERT INTO books_master (book_id, book_name, author_name) VALUES (?, ?, ?)",
    (book_id, book_name, author_name)
)
connection.commit()

cursor.execute("SELECT * FROM books_master")
for row in cursor.fetchall():
    print(row)

connection.close()
```

If the examiner specifically requires MySQL, use the MySQL connector and the database credentials provided by the lab instead of this SQLite example.

## Practical 14 — Socket Programming (Client and Server)

1. Import Python's `socket` module.
2. For a server, create a socket, bind it to a host and port, call `listen()`, and wait with `accept()`.
3. For a client, create a socket and call `connect()` using the server host and port.
4. Send or receive data using `sendall()` and `recv()`.
5. Close the connection.
6. Run the server in one terminal first; run the client in a second terminal.
7. Viva: host identifies the machine, port identifies the application endpoint, `bind()` assigns the server address, `listen()` waits for clients, `accept()` accepts a connection, and `connect()` starts a client connection.

Server (`server.py`):

```python
import socket

host = "127.0.0.1"
port = 5000

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind((host, port))
server.listen(1)
print("Waiting for client...")

connection, address = server.accept()
print("Connected by:", address)

connection.sendall(b"Hello from server")
connection.close()
server.close()
```

Client (`client.py`):

```python
import socket

host = "127.0.0.1"
port = 5000

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect((host, port))
data = client.recv(1024)
print("Received:", data.decode())
client.close()
```

Run `python server.py` in one terminal, then `python client.py` in another.

## Practical 15 — Django Student Information CRUD

Goal: create a web app to add, view, update, and delete student records.

### Exam steps

1. Open a terminal in the project folder.
2. Create and activate a virtual environment if required.
3. Install Django if it is not installed: `python -m pip install django`.
4. Create a project: `django-admin startproject config .`
5. Create an app: `python manage.py startapp students`.
6. Add `'students'` to `INSTALLED_APPS` in `config/settings.py`.
7. In `students/models.py`, create a `Student` model with fields such as `name`, `roll_no`, and `email`.
8. Run `python manage.py makemigrations` and `python manage.py migrate`.
9. Register the model in `students/admin.py` if you want to manage records through Django admin.
10. Create views in `students/views.py` for listing students, adding a student, editing a student, and deleting a student.
11. Create `students/urls.py` and map URL paths to those views.
12. Include the app URLs in the main `config/urls.py`.
13. Create HTML templates under `students/templates/students/` for the list and form pages.
14. Run `python manage.py runserver` and open `http://127.0.0.1:8000/`.
15. Test all four actions: add, view, update, delete.

### Core model example

```python
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    roll_no = models.CharField(max_length=20, unique=True)
    email = models.EmailField()

    def __str__(self):
        return self.name
```

### Key viva points

- `models.py`: defines database models.
- `views.py`: handles requests and returns responses.
- `urls.py`: maps URL patterns to views.
- Templates: HTML pages rendered by Django.
- `makemigrations`: creates migration files from model changes.
- `migrate`: applies migrations to the database.
- CRUD: Create, Read, Update, Delete.
- `runserver`: starts Django's local development server.

If a project is already supplied in the lab, do not create a new project over it. Open the folder containing `manage.py`, install its requirements if provided, run migrations if appropriate, and start the server. Follow the project's README and lab setup instructions.

## Practical 16 — Web Application Development in Joomla

The syllabus asks for one Joomla web application, chosen from Library Management, Online Banking, Placement Portal, Human Resource Management, Hospital Management, or Electronics Shopping. The exact steps depend on the installed Joomla version and any extensions provided by the lab.

### Exam steps

1. Open the Joomla site supplied by the lab, or install Joomla only if the exam requires it and time permits.
2. Open the administrator panel, usually at `/administrator`.
3. Sign in with the lab-provided administrator credentials.
4. Choose the assigned application topic.
5. Create categories to organize related content.
6. Create the required articles/pages using Content → Articles (menu names may vary by Joomla version).
7. Add a menu item for each important page and set the Home page/default page if required.
8. Use the template/style settings to select or configure the site's appearance.
9. Upload and organize images/media using the Media area.
10. Install only the extensions supplied or permitted by the lab; configure them if needed.
11. Preview the site on the front end.
12. Test the navigation links and verify that each page displays correctly.
13. Viva: Joomla is a content management system (CMS); articles hold content, categories organize content, menus provide navigation, templates control appearance, and extensions add functionality.

Important: a fully working online banking or hospital-management system needs specialized components and security. In a practical exam, build the pages and functionality actually specified by your instructor; do not represent a simple set of pages as a secure production banking system.
