# Backend Web Development — Express & MongoDB REST API

A backend RESTful API built with **Node.js**, **Express.js**, **MongoDB Atlas**, and **Mongoose**. This project implements user authentication (register, login, logout) with password hashing and complete CRUD (Create, Read, Update, Delete) operations for character posts.

---

## 🚀 Technologies Used

- **Runtime Environment:** [Node.js](https://nodejs.org/) (ES Modules)
- **Framework:** [Express.js](https://expressjs.com/)
- **Database:** [MongoDB Atlas](https://www.mongodb.com/atlas) (Cloud NoSQL Database)
- **ODM (Object Data Modeling):** [Mongoose](https://mongoosejs.com/)
- **Security:** [bcrypt](https://www.npmjs.com/package/bcrypt) (Salted password hashing)
- **API Testing:** [Postman](https://www.postman.com/)
- **Development Tooling:** Nodemon, Dotenv

---

## 🏛️ Architecture & Project Structure

The project follows the **Model-Route-Controller (MVC)** architectural pattern:

```text
backend/
├── src/
│   ├── config/
│   │   ├── constants.js          # App constants (DB name)
│   │   └── database.js           # Resilient MongoDB Atlas connection logic
│   ├── controllers/
│   │   ├── user.controller.js    # Auth logic (register, login, logout)
│   │   └── post.controller.js    # CRUD logic (create, read, update, delete)
│   ├── models/
│   │   ├── user.model.js         # Mongoose User Schema & bcrypt pre-save hook
│   │   └── post.model.js         # Mongoose Character Post Schema
│   ├── routes/
│   │   ├── user.route.js         # Express routers for auth endpoints
│   │   └── post.route.js         # Express routers for post endpoints
│   ├── app.js                    # Express app configuration & middleware
│   └── index.js                  # Server entry point & port listener
├── .env.example                  # Environment variable template
├── .gitignore                    # Excludes node_modules & secrets (.env)
└── package.json                  # Dependencies and scripts
postman/
├── Users.postman_collection.json # Ready-to-import Auth collection
└── CRUD.postman_collection.json  # Ready-to-import CRUD collection
```

### Request Flow
1. **Request:** A client (e.g. Postman) sends an HTTP request to `http://localhost:4000/api/v1/...`
2. **Middleware:** `express.json()` in `app.js` parses the incoming JSON request body.
3. **Router:** The route directs the request to the matching controller function.
4. **Controller:** The controller validates the input and interacts with MongoDB via Mongoose Models.
5. **Database:** Mongoose queries/mutates data in the MongoDB Atlas cloud cluster.
6. **Response:** The controller returns an appropriate HTTP status code (200, 201, 400, 404, 500) and a structured JSON payload.

---

## ⚙️ Setup and Installation

### Prerequisites
- [Node.js](https://nodejs.org/) (v18 or higher)
- A free [MongoDB Atlas](https://www.mongodb.com/atlas) account

### 1. Clone & Install Dependencies
Navigate into the `backend` folder and install dependencies:
```bash
cd backend
npm install
```

### 2. Configure Environment Variables
Create a `.env` file in the `backend/` directory:
```env
MONGODB_URL=mongodb+srv://Prince_Kumar:17082005@cluster0.wffvoqr.mongodb.net/
PORT=4000
```

### 3. Start the Server
For development with auto-reloading:
```bash
npm run dev
```

When connected successfully, you will see:
```text
Connecting to MongoDB (1/5)...
MongoDB connected!!! Host: ac-gp1urfx-shard-00-00.wffvoqr.mongodb.net
Server is running on port 4000
```

---

## 📡 API Endpoints Reference

Base URL: `http://localhost:4000`

### 1. Authentication Endpoints (`/api/v1/users`)

| Method | Endpoint | Description | Status Code |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/v1/users/register` | Registers a new user (hashes password) | `201 Created` |
| `POST` | `/api/v1/users/login` | Validates email & password via bcrypt | `200 OK` |
| `POST` | `/api/v1/users/logout` | Validates user & logs them out | `200 OK` |

#### Sample Registration Body (`POST /api/v1/users/register`):
```json
{
  "username": "prince_kumar",
  "email": "princekumar@example.com",
  "password": "SecurePass123"
}
```

---

### 2. Character Post CRUD Endpoints (`/api/v1/posts`)

| Method | Endpoint | Description | Status Code |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/v1/posts/create` | Creates a new character post | `201 Created` |
| `GET` | `/api/v1/posts/getPosts` | Retrieves all character posts | `200 OK` |
| `PATCH` | `/api/v1/posts/update/:id` | Partially updates a post by its ID | `200 OK` |
| `DELETE` | `/api/v1/posts/delete/:id` | Deletes a post by its ID | `200 OK` |

#### Sample Create Post Body (`POST /api/v1/posts/create`):
```json
{
  "name": "Kina",
  "description": "A skilled martial artist",
  "age": 26
}
```

---

## 🧪 Testing with Postman

1. Open Postman and click **Import**.
2. Select the two JSON files located in the `postman/` folder:
   - `Users.postman_collection.json`
   - `CRUD.postman_collection.json`
3. Send requests in order:
   - Register a user → verify password is not returned in plain text.
   - Login with the registered email and password.
   - Create a character post → copy the generated `_id` from the response.
   - Run `Get All Posts` to view all documents.
   - Update and Delete the post using the copied `_id`.

---

## 🔒 Security Highlights

- **Password Hashing:** Passwords are never stored in plain text. Mongoose `pre('save')` hook hashes them using `bcrypt` with 10 salt rounds.
- **Environment Isolation:** Sensitive database credentials and server ports are isolated in `.env` and excluded from source control via `.gitignore`.
- **Validation:** Both email and username uniqueness are verified before document creation to prevent duplicate key collisions.

---

## 👨‍💻 Author

**Prince Kumar**  
Built as an introductory backend development project following Shivani's Node.js & Express tutorial (Hack Club).
