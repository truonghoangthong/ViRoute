# ViRoute

A web application for bus ticket booking with real-time route tracking. Users can search routes, book and manage tickets, and save favorite places, while the platform handles authentication and payment records.

## Authors

| Name | GitHub | Role |
|------|--------|------|
| Khoi Do | [@khoidm2004](https://github.com/khoidm2004) | Project Manager, Tester, Deploy |
| Dung Nguyen | [@pingviini314159](https://github.com/pingviini314159) | Backend Dev |
| Thong Truong | [@truonghoangthong](https://github.com/truonghoangthong) | Backend Dev |
| Thang Tran | [@tranthangok](https://github.com/tranthangok) | Designer, Frontend Dev |
| Nhi Nguyen | [@nhingnguyen](https://github.com/nhingnguyen) | Designer, Frontend Dev |

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Django 5.1.2, Django REST Framework |
| Auth | GitHub OAuth (django-allauth), JWT (djangorestframework-simplejwt) |
| Database | MySQL |
| Deployment | Railway, Gunicorn |
| External API | OpenRouteService (route and map data) |

## Features

- User registration and login with bcrypt password hashing
- GitHub OAuth authentication
- JWT-based API authentication
- Bus route search by departure and destination point
- Ticket management
- Favorite places per user
- Avatar upload and retrieval
- Password reset via email

---

## Project Structure

```
ViRoute/
├── viroute/
│   ├── manage.py
│   ├── Procfile                   # Gunicorn entry point for Railway
│   ├── requirements.txt
│   ├── fake_data.py               # Seed script for generating fake data
│   ├── save_images.py             # Script to seed image records into DB
│   ├── users.csv                  # Sample generated user data
│   ├── viroute/                   # Django project config
│   │   ├── settings.py
│   │   ├── urls.py
│   │   ├── wsgi.py
│   │   └── asgi.py
│   └── virouteapp/                # Main Django app
│       ├── models.py
│       ├── views.py
│       ├── serializers.py
│       ├── urls.py
│       ├── admin.py
│       ├── migrations/
│       ├── templates/             # HTML templates (password reset flow)
│       └── management/commands/  # Custom Django management commands
├── test.py                        # Manual API test script
└── .gitignore
```

---

## Requirements

- Python 3.x
- MySQL

---

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

### 5. Configure environment variables

Create a `.env` file in the `viroute/` directory and set the following:

```env
SECRET_KEY=your_django_secret_key
DB_NAME=viroute
DB_USER=root
DB_PASSWORD=your_password
DB_HOST=127.0.0.1
DB_PORT=3306
EMAIL_HOST_USER=your_email@gmail.com
EMAIL_HOST_PASSWORD=your_app_password
```

Then update `viroute/settings.py` to read from environment variables instead of hardcoded values.

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

---

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

---

## Database Models

| Model | Description |
|-------|-------------|
| `User` | App user with hashed password, avatar, balance, and citizenship |
| `Account` | Linked account tracking payment, purchase, and top-up history |
| `Ticket` | Bus or metro ticket with departure/destination info and pricing |
| `Bus` / `Metro` | Vehicle models with route and plate number |
| `BusRoute` | Named route with start and end points |
| `FavPlace` | A user's saved favorite location |
| `Image` | Image records linked to file paths in media storage |

---

## Resetting the Database

If you need to reset the database after changing models:

```sql
DROP DATABASE viroute;
CREATE DATABASE viroute;
```

```bash
python manage.py makemigrations
python manage.py migrate
python fake_data.py
```

---

## Deployment

The app is configured for deployment on [Railway](https://railway.app/) using Gunicorn.

The `Procfile` entry:

```
web: gunicorn viroute.wsgi:application
```

Static files are served via WhiteNoise. Make sure `ALLOWED_HOSTS` and `CSRF_TRUSTED_ORIGINS` are updated with the production domain before deploying.

---

## Notes

- `DEBUG = True` and a hardcoded `SECRET_KEY` are present in `settings.py` — these must be changed before any production deployment.
- The `users.csv` file generated by `fake_data.py` contains plaintext passwords and should never be committed to a public repository.

---
