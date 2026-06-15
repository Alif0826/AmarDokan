# Amar Dokan — Online Shop Management System

A full-stack e-commerce web application for browsing and purchasing clothing products, built as a Software Engineering course project. The system supports customer registration with OTP verification, JWT-based authentication, product catalog management, cart functionality, and order placement.

---

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Backend Setup](#backend-setup)
  - [Frontend Setup](#frontend-setup)
- [API Reference](#api-reference)
- [Database Schema](#database-schema)
- [Screenshots](#screenshots)
- [Known Issues & Limitations](#known-issues--limitations)

---

## Features

**Customer**
- Register with name, username, email, and mobile number
- Email OTP verification to activate account
- JWT-based login with 30-minute token expiry
- Browse products by category (Men, Women, Kids, Others) with subcategory filters
- Add products to cart, adjust quantities, and place orders
- View order history and manage profile
- Forgot password flow with OTP re-verification

**Admin / Delivery**
- Add, update, and delete products with image upload
- Delivery man login and assigned task lookup by username

---

## Tech Stack

| Layer     | Technology                                      |
|-----------|-------------------------------------------------|
| Frontend  | React 18, React Router v6, Axios, Lucide React  |
| Backend   | FastAPI (Python), SQLAlchemy ORM, Jose (JWT)    |
| Database  | SQLite (`amardokan.db`)                         |
| Auth      | bcrypt password hashing, JWT (HS256)            |
| Email     | Python `smtplib` via Gmail SMTP                 |
| Styling   | CSS Modules, custom CSS                         |

---

## Project Structure

```
Software-Engineering-main/
│
├── # Backend (Python / FastAPI)
├── main.py             # All API route handlers
├── models.py           # SQLAlchemy ORM models
├── database.py         # DB engine and session setup
├── schemas.py          # Pydantic request/response schemas
│
├── # Frontend (React / Vite)
├── main.jsx            # React entry point with RouterProvider
├── App.jsx             # Root component
│
├── # Routes
├── MainRoute.jsx       # React Router browser router config
│
├── # Components
├── Navbar.jsx          # Top navigation with dropdowns and cart icon
├── Front.jsx           # Homepage / landing page
├── Product.jsx         # Product listing with category filter
├── ProductList.jsx     # Admin product list view
├── SingleProduct.jsx   # Individual product card
├── AddToCart.jsx       # Cart context, provider, and cart sidebar
├── Login.jsx           # Login page
├── LoginPanel.jsx      # Slide-in login panel
├── Signup.jsx          # Registration page
├── RegistrationPanel.jsx # Slide-in registration panel
├── Otp.jsx             # OTP verification page
├── Profile.jsx         # User profile page
├── OrderList.jsx       # Customer order history
├── OrderDetails.jsx    # Individual order details
├── About.jsx           # About page
│
├── # Layout
├── MainLayout.jsx      # Shared layout wrapper (Navbar + outlet)
│
├── # Config / Utilities
├── client.js           # Axios instance with base URL
│
├── # Styles
├── App.css, index.css, New.css, front.css
├── Login.module.css, Signup.module.css
├── Product.module.css, About.module.css, Otp.module.css
│
├── # Documentation
├── Amar_Dokan_Project_Report.pdf
├── Project proposal.pdf
├── SDLC model selection matrix.pdf
├── DFD of project.docx
├── Sequence Diagram of project.pdf
├── Project Requirements.docx
│
└── # Assets (images, logos, etc.)
    └── *.jpg, *.png, *.jpeg
```

---

## Getting Started

### Prerequisites

- Python 3.9+
- Node.js 18+ and npm
- A Gmail account for OTP email sending (App Password required)

---

### Backend Setup

1. **Clone the repository and navigate to the project folder.**

2. **Install Python dependencies:**

   ```bash
   pip install fastapi uvicorn sqlalchemy python-jose bcrypt python-multipart
   ```

3. **Configure email credentials** in `main.py`:

   ```python
   from_mail = 'your-email@gmail.com'
   server.login(from_mail, 'your-app-password')
   ```

   > Generate a Gmail App Password at [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords). Do **not** use your regular Gmail password.

4. **(Optional) Rotate the secret key** in `main.py` for production:

   ```python
   SECRET_KEY = "your-own-random-secret-key"
   ```

5. **Run the backend server:**

   ```bash
   uvicorn main:app --reload
   ```

   The API will be available at `http://127.0.0.1:8000`.  
   Interactive docs (Swagger UI) are at `http://127.0.0.1:8000/docs`.

---

### Frontend Setup

1. **Install dependencies:**

   ```bash
   npm install
   ```

2. **Start the development server:**

   ```bash
   npm run dev
   ```

   The app will be available at `http://localhost:5173`.

> Make sure the backend is running before starting the frontend. The Axios client is pre-configured to point to `http://127.0.0.1:8000`.

---

## API Reference

### User Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/user_registration/` | Register a new user and send OTP |
| `GET` | `/user_login/{user_name}/{password}` | Log in and receive a JWT access token |
| `PUT` | `/user_update/` | Update user profile details |
| `POST` | `/delete_user_account/` | Delete a user account |

### OTP Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/verify_otp/{email}/{otp}` | Verify OTP and activate account |
| `GET` | `/resend_otp/{email}` | Resend a new OTP to the user's email |

### Password Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/forget_password/` | Reset password and send OTP for re-verification |

### Product Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/product/` | Add a new product with image upload |
| `GET` | `/products/` | Get all products |
| `GET` | `/product/{product_id}/` | Get a single product by ID |
| `GET` | `/product/{product_id}/image` | Stream the product image |
| `PUT` | `/product/{product_id}/` | Update product details or image |
| `DELETE` | `/product/{product_id}/` | Delete a product |

### Order Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/order/` | Place an order for a product |

### Delivery Man Management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/delivery_man_login/{user_id}/{password}` | Delivery man login |
| `GET` | `/delivery_man_assigned_task/{user_name}` | Get task details for a delivery man by customer username |

---

## Database Schema

The SQLite database (`amardokan.db`) is created automatically on first run.

| Table | Key Columns |
|-------|-------------|
| `user_registration` | `id`, `name`, `user_name`, `email`, `mobile_number`, `is_active`, `user_id`, `salt` |
| `login` | `id`, `user_id` (hashed), `password` (hashed), `role` |
| `user_otp` | `id`, `email`, `otp` |
| `products` | `id`, `name`, `image` (binary), `price`, `description`, `category` |
| `Order` | `order_id`, `user_name`, `product_id`, `quantity`, `price`, `address` |

---

## Known Issues & Limitations

- **Credentials in source code** — the Gmail password and JWT secret key are hardcoded in `main.py`. These must be moved to environment variables before any deployment.
- **Login endpoint mismatch** — the frontend `Login.jsx` calls `/login` but the backend defines `/user_login/{user_name}/{password}`. This needs to be aligned.
- **No role-based access control on the frontend** — admin product management routes are not protected by a role check in the UI.
- **SQLite for development only** — SQLite is not suitable for concurrent production workloads. Migrate to PostgreSQL or MySQL for deployment.
- **OTP is plaintext** — OTP values are stored and compared as plaintext strings in the database.
- **Image storage in DB** — product images are stored as binary blobs directly in SQLite, which does not scale well. Consider file-based or object storage (e.g., S3) for production.
