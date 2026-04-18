# ViRoute

ViRoute is a web app for easy bus bookings with real-time tracking. Users can book, pay, and track their rides, while operators manage routes and schedules on a single platform.

## Authors

- [Khoi Do](https://github.com/khoidm2004) — Project Manager, Tester, Deploy
- [Dung Nguyen](https://github.com/pingviini314159) — Backend Dev
- [Thong Truong](https://github.com/truonghoangthong) — Backend Dev
- [Thang Tran](https://github.com/tranthangok) — Designer, Frontend Dev
- [Nhi Nguyen](https://github.com/nhingnguyen) — Designer, Frontend Dev

## Tech Stack

- **Backend:** Django 5.1.2, Django REST Framework
- **Auth:** GitHub OAuth (django-allauth), JWT (djangorestframework-simplejwt)
- **Database:** MySQL
- **Deployment:** Railway, Gunicorn

## Features

- User registration and login with bcrypt password hashing
- GitHub OAuth authentication
- JWT-based API authentication
- Bus route search by start/end point
- Ticket management
- Favorite places per user
- Avatar upload and retrieval
- Password reset via email
- OpenRouteService API integration for route/map data

## Requirements

- Python 3.x
- MySQL

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/khoidm2004/ViRoute
cd ViRoute
git checkout Backend
cd viroute
```

### 2. Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate       # macOS/Linux
venv\Scripts\activate          # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Set up the MySQL database

```sql
CREATE DATABASE viroute;
CREATE USER 'root'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON viroute.* TO 'root'@'localhost';
FLUSH PRIVILEGES;
```

### 5. Configure the database connection

Update `viroute/settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'viroute',
        'USER': 'root',
        'PASSWORD': 'your_password',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```

### 6. Run migrations

```bash
python manage.py migrate
```

### 7. (Optional) Seed fake data

```bash
python fake_data.py
```

### 8. Create a superuser

```bash
python manage.py createsuperuser
```

### 9. Start the development server

```bash
python manage.py runserver
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/login/` | Login with email and password |
| `POST` | `/signup/` | Register a new user |
| `PUT` | `/update_user/<user_id>/` | Update user information |
| `GET` | `/tickets/` | List all tickets |
| `GET` | `/get_image/<image_name>/` | Get image by name |
| `GET` | `/api/bus_routes/` | Get all bus routes |
| `POST` | `/api/bus_routes/filter/` | Filter bus routes by start and end point |
| `POST` | `/fav-place/create/` | Create a favorite place |
| `GET` | `/fav-place/<user_id>/` | Get favorite places for a user |
| `PUT` | `/update-avatar/` | Update user avatar |
| `GET` | `/get-avatar-url/` | Get avatar URL of the current user |
| `POST` | `/auth/forgot_password/` | Request a password reset email |

## Resetting the Database

If you need to change model fields and reset the database from scratch:

```sql
DROP DATABASE viroute;
CREATE DATABASE viroute;
```

```bash
python manage.py makemigrations
python manage.py migrate
python fake_data.py
```

