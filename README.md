# 🛒 Full E-Commerce REST API — Node.js

A full-featured, production-ready **E-Commerce REST API** built with **Node.js**, **Express**, and **MongoDB**. It covers everything an online store needs — from product catalog management to secure checkout with Stripe.

---

## ✨ Features

- 🔐 **JWT Authentication & Authorization** — Signup, login, role-based access control (user / manager / admin)
- 🔑 **Password Management** — Forgot password flow with email OTP, code verification, and reset
- 🛍️ **Product Catalog** — Full CRUD for categories, subcategories, brands, and products with image upload & processing
- ⭐ **Reviews & Ratings** — Users can post reviews; average ratings are recalculated automatically
- ❤️ **Wishlist** — Add/remove products from a personal wishlist
- 📍 **Address Book** — Store and manage multiple shipping addresses per user
- 🛒 **Shopping Cart** — Add items, apply discount coupons, view totals
- 🏷️ **Coupons** — Create and apply discount coupons to cart
- 📦 **Order Management** — Cash-on-delivery and card (Stripe) payment flows
- 💳 **Stripe Integration** — Secure online payment with webhook support
- 🔍 **Advanced Query Features** — Filtering, sorting, field selection, keyword search, and pagination
- 🛡️ **Security Hardening** — Rate limiting, HTTP parameter pollution protection, CORS, request body limits
- 🗜️ **Compression** — Gzip response compression for better performance
- 📝 **Request Validation** — Input validation using `express-validator`

---

## 🏗️ Project Structure

```
├── config/
│   └── database.js               # MongoDB connection setup
├── middlewares/
│   ├── errorMiddleware.js        # Global error handler
│   ├── uploadImageMiddleware.js  # Multer file upload config
│   └── validatorMiddleware.js   # Validation result handler
├── models/
│   ├── userModel.js
│   ├── productModel.js
│   ├── categoryModel.js
│   ├── subCategoryModel.js
│   ├── brandModel.js
│   ├── reviewModel.js
│   ├── cartModel.js
│   ├── couponModel.js
│   └── orderModel.js
├── routes/
│   ├── index.js             # Route mounting entry point
│   ├── authRoute.js
│   ├── userRoute.js
│   ├── categoryRoute.js
│   ├── subCategoryRoute.js
│   ├── brandRoute.js
│   ├── productRoute.js
│   ├── reviewRoute.js
│   ├── wishlistRoute.js
│   ├── addressRoute.js
│   ├── couponRoute.js
│   ├── cartRoute.js
│   └── orderRoute.js
├── services/                # Business logic / controllers
│   ├── handlersFactory.js   # Generic CRUD factory
│   ├── authService.js
│   ├── userService.js
│   ├── categoryService.js
│   ├── subCategoryService.js
│   ├── brandService.js
│   ├── productService.js
│   ├── reviewService.js
│   ├── wishlistService.js
│   ├── addressService.js
│   ├── couponService.js
│   ├── cartService.js
│   └── orderService.js
├── utils/
│   ├── apiError.js          # Custom error class
│   ├── apiFeatures.js       # Filter / Sort / Paginate / Search
│   ├── createToken.js       # JWT token generator
│   ├── sendEmail.js         # Nodemailer email sender
│   └── validators/          # Input validation rules
├── uploads/                 # Static uploaded images
├── server.js                # App entry point
└── config.env               # Environment variables (not committed)
```

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) `>= 16.13.0`
- [MongoDB](https://www.mongodb.com/) (local or Atlas)
- [Stripe account](https://stripe.com/) (for online payments)
- SMTP credentials (e.g. Gmail, Mailtrap) for emails

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/nodejs-ecommerce-api.git
cd nodejs-ecommerce-api

# 2. Install dependencies
npm install

# 3. Create the environment config file
cp config.env.example config.env
# Then fill in your values (see Environment Variables section below)

# 4. Start the development server
npm run start:dev
```

---

## ⚙️ Environment Variables

Create a `config.env` file in the root directory with the following keys:

```env
# Server
NODE_ENV=development
PORT=8000
BASE_URL=http://localhost:8000

# MongoDB
DB_URI=mongodb://localhost:27017/ecommerce-db

# JWT
JWT_SECRET_KEY=your_jwt_secret_key
JWT_EXPIRE_TIME=90d

# Email (Nodemailer)
EMAIL_HOST=smtp.mailtrap.io
EMAIL_PORT=587
EMAIL_USER=your_email_user
EMAIL_PASSWORD=your_email_password
EMAIL_FROM=noreply@eshop.com

# Stripe
STRIPE_SECRET=sk_test_your_stripe_secret_key
STRIPE_WEBHOOK_SECRET=whsec_your_stripe_webhook_secret
```

---

## 📜 Scripts

| Command | Description |
|---|---|
| `npm run start:dev` | Start the server in development mode with **nodemon** |
| `npm start` | Start the server in production mode |

---

## 📡 API Endpoints

All endpoints are prefixed with `/api/v1`.

### 🔐 Authentication — `/api/v1/auth`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `POST` | `/signup` | Register a new user | Public |
| `POST` | `/login` | Login and receive JWT | Public |
| `POST` | `/forgotPassword` | Send a 6-digit reset code via email | Public |
| `POST` | `/verifyResetCode` | Verify the password reset code | Public |
| `PUT` | `/resetPassword` | Reset password using verified code | Public |

### 👤 Users — `/api/v1/users`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | List all users | Admin |
| `POST` | `/` | Create a new user | Admin |
| `GET` | `/:id` | Get a specific user | Admin |
| `PUT` | `/:id` | Update a user | Admin |
| `DELETE` | `/:id` | Delete a user | Admin |
| `GET` | `/getMe` | Get logged-in user profile | User |
| `PUT` | `/updateMe` | Update logged-in user profile | User |
| `PUT` | `/changeMyPassword` | Change logged-in user password | User |
| `DELETE` | `/deleteMe` | Deactivate logged-in user | User |

### 🗂️ Categories — `/api/v1/categories`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get all categories | Public |
| `POST` | `/` | Create a category | Admin / Manager |
| `GET` | `/:id` | Get a category | Public |
| `PUT` | `/:id` | Update a category | Admin / Manager |
| `DELETE` | `/:id` | Delete a category | Admin |
| `GET` | `/:categoryId/subcategories` | Get subcategories of a category | Public |

### 🗃️ Subcategories — `/api/v1/subcategories`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get all subcategories | Public |
| `POST` | `/` | Create a subcategory | Admin / Manager |
| `GET` | `/:id` | Get a subcategory | Public |
| `PUT` | `/:id` | Update a subcategory | Admin / Manager |
| `DELETE` | `/:id` | Delete a subcategory | Admin |

### 🏷️ Brands — `/api/v1/brands`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get all brands | Public |
| `POST` | `/` | Create a brand | Admin / Manager |
| `GET` | `/:id` | Get a brand | Public |
| `PUT` | `/:id` | Update a brand | Admin / Manager |
| `DELETE` | `/:id` | Delete a brand | Admin |

### 📦 Products — `/api/v1/products`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get all products | Public |
| `POST` | `/` | Create a product | Admin / Manager |
| `GET` | `/:id` | Get a product | Public |
| `PUT` | `/:id` | Update a product | Admin / Manager |
| `DELETE` | `/:id` | Delete a product | Admin |

### ⭐ Reviews — `/api/v1/reviews`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get all reviews | Public |
| `POST` | `/` | Create a review | User |
| `GET` | `/:id` | Get a review | Public |
| `PUT` | `/:id` | Update own review | User |
| `DELETE` | `/:id` | Delete a review | User / Admin |

### ❤️ Wishlist — `/api/v1/wishlist`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get logged-in user wishlist | User |
| `POST` | `/` | Add product to wishlist | User |
| `DELETE` | `/:productId` | Remove product from wishlist | User |

### 📍 Addresses — `/api/v1/addresses`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get user saved addresses | User |
| `POST` | `/` | Add a new address | User |
| `DELETE` | `/:addressId` | Remove an address | User |

### 🏷️ Coupons — `/api/v1/coupons`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get all coupons | Admin / Manager |
| `POST` | `/` | Create a coupon | Admin / Manager |
| `GET` | `/:id` | Get a coupon | Admin / Manager |
| `PUT` | `/:id` | Update a coupon | Admin / Manager |
| `DELETE` | `/:id` | Delete a coupon | Admin |

### 🛒 Cart — `/api/v1/cart`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `GET` | `/` | Get logged-in user cart | User |
| `POST` | `/` | Add item to cart | User |
| `PUT` | `/:itemId` | Update item quantity | User |
| `DELETE` | `/:itemId` | Remove item from cart | User |
| `DELETE` | `/` | Clear the cart | User |
| `PUT` | `/applyCoupon` | Apply a discount coupon | User |

### 📬 Orders — `/api/v1/orders`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| `POST` | `/:cartId` | Create a cash order | User |
| `GET` | `/` | Get all orders | User / Admin / Manager |
| `GET` | `/:id` | Get a specific order | User / Admin / Manager |
| `PUT` | `/:id/pay` | Mark order as paid | Admin / Manager |
| `PUT` | `/:id/deliver` | Mark order as delivered | Admin / Manager |
| `GET` | `/checkout-session/:cartId` | Create Stripe checkout session | User |

### 💳 Stripe Webhook

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/webhook-checkout` | Stripe payment confirmation webhook |

---

## 🔍 Advanced Query Parameters

All list endpoints support the following query parameters:

| Parameter | Description | Example |
|---|---|---|
| `page` | Page number | `?page=2` |
| `limit` | Results per page (default: 50) | `?limit=10` |
| `sort` | Sort by field(s), comma-separated | `?sort=-price,ratingsAverage` |
| `fields` | Select specific fields | `?fields=title,price` |
| `keyword` | Search by name/title/description | `?keyword=laptop` |
| `price[gte]` | Filter by price range | `?price[gte]=100&price[lte]=500` |
| `ratingsAverage[gte]` | Filter by rating | `?ratingsAverage[gte]=4` |

---

## 🛡️ Security Measures

| Feature | Implementation |
|---|---|
| Password hashing | `bcryptjs` with salt rounds of 12 |
| JWT authentication | `jsonwebtoken` with expiry |
| Rate limiting | `express-rate-limit` — 100 req / 15 min per IP |
| HTTP Param Pollution | `hpp` middleware protection |
| CORS | Enabled for all origins via `cors` |
| Body size limit | JSON body capped at `20kb` |
| Async error handling | `express-async-handler` throughout |

---

## 🧰 Tech Stack

| Category | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MongoDB + Mongoose |
| Authentication | JSON Web Tokens (JWT) |
| Password Hashing | bcryptjs |
| File Upload | Multer |
| Image Processing | Sharp |
| Email | Nodemailer |
| Payment | Stripe |
| Validation | express-validator |
| Linting | ESLint (Airbnb config) + Prettier |
| Dev Server | Nodemon |


