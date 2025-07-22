## 🧠 **What is Node.js?**

- an **open-source**, **cross-platform**, **runtime environment** that lets you run **JavaScript on the server-side** using the **V8 engine (from Chrome)**.

### ✅ Key Features:

- **Non-blocking I/O model (asynchronous)**
- **Single-threaded event loop**
- **Built-in libraries for HTTP, file system, streams, etc.**
- **NPM (Node Package Manager)** for dependency management
- **Highly scalable** for I/O-bound applications

### ✅ Why use Node.js?

| Feature           | Benefit                                                 |
| ----------------- | ------------------------------------------------------- |
| 🧵 Event-driven   | Handles thousands of concurrent connections efficiently |
| ⚡ Fast execution | Powered by V8 engine                                    |
| 🌐 Full-stack JS  | Use JavaScript both on frontend & backend               |
| 🧩 Rich ecosystem | Over 2M packages on NPM                                 |
| 🔁 Real-time      | Great for apps like chat, live dashboards               |

---

## 🔧 **What is Express.js?**

### ✅ Definition:

a **minimal**, **flexible web application framework** for Node.js that simplifies building web applications and RESTful APIs.

### ✅ Why use Express?

| Feature               | Benefit                                        |
| --------------------- | ---------------------------------------------- |
| 📦 Lightweight        | Adds routing, middleware, and abstraction      |
| 🔁 Middleware support | Plug logic between request and response        |
| 📡 Routing            | Simple API to manage routes                    |
| 🧱 Extensible         | Use any template engine, ORM, database         |
| ⚙️ Easy integration   | Works well with any front-end or mobile client |

---

## 🏗️ **Node.js & Express.js Architecture**

```
[Client Browser] → HTTP Request
      ↓
[Express.js Router] → Middleware (auth, validation)
      ↓
[Controller / Route Handler]
      ↓
[Service / DB Layer] ← MongoDB/MySQL
      ↓
Response → [Client]
```

---

## 🛠️ **Installing Node.js & Express**

```bash
# Install Node.js from https://nodejs.org

# Initialize Node project
npm init -y

# Install Express
npm install express
```

---

## 🔧 **Scalable Node.js + Express.js Folder Structure (MVC + Service Layer)**

✅ Benefits:

- **Scalability**: Easy to maintain as app grows
- **Separation of concerns**: Clean architecture (Routes, Logic, DB, etc.)
- **Testability**: Easy to write unit/integration tests
- **Reusability**: Services and utils can be reused
- **Team collaboration**: Consistent structure for all devs

---

## 📂 **Recommended Folder Structure**

```
project-root/
├── server.js                # App entry point
├── config/                  # Environment, DB, constants
│   ├── db.js
│   └── env.js
├── routes/                  # Route definitions (HTTP endpoints)
│   └── user.routes.js
├── controllers/            # Handle req/res, call services
│   └── user.controller.js
├── services/               # Business logic layer
│   └── user.service.js
├── models/                 # Mongoose or DB schemas
│   └── user.model.js
├── middleware/             # Custom middlewares (auth, error)
│   └── auth.js
├── utils/                  # Helper functions/utilities
│   └── token.js
├── validators/             # Input validation
│   └── user.validator.js
├── logs/                   # App logs if needed
├── tests/                  # Unit/integration tests
├── .env                    # Environment variables
├── .gitignore
├── package.json
```

---

## 🚀 **Walkthrough + Coding Example**

Let’s say you’re building a **User Auth System (Register/Login/Profile)**

---

### ✅ `server.js` — Entry Point

```js
require("dotenv").config();
const express = require("express");
const connectDB = require("./config/db");
const userRoutes = require("./routes/user.routes");
const app = express();

connectDB(); // connect to DB

app.use(express.json()); // parse JSON body

// Routes
app.use("/api/users", userRoutes);

// Global error handler (optional)
app.use(require("./middleware/errorHandler"));

app.listen(process.env.PORT || 5000, () =>
  console.log(`Server running on port ${process.env.PORT}`)
);
```

---

### ✅ Create the error handler middleware

```js
// middleware/errorHandler.js

module.exports = (err, req, res, next) => {
  console.error(err.stack); // log error (optional)

  const statusCode = err.statusCode || 500;
  const message = err.message || "Internal Server Error";

  res.status(statusCode).json({
    success: false,
    message,
    // optionally include stack trace only in dev
    stack: process.env.NODE_ENV === "production" ? null : err.stack,
  });
};
```

---

### ✅ `config/db.js` — MongoDB Connection

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB Connected");
  } catch (err) {
    console.error(err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

---

### ✅ `models/user.model.js`

```js
const mongoose = require("mongoose");

const UserSchema = new mongoose.Schema(
  {
    name: { type: String, required: true },
    email: { type: String, unique: true, required: true },
    password: { type: String, required: true },
  },
  { timestamps: true }
);

module.exports = mongoose.model("User", UserSchema);
```

---

### ✅ `routes/user.routes.js`

```js
const express = require("express");
const {
  registerUser,
  loginUser,
  getProfile,
} = require("../controllers/user.controller");
const auth = require("../middleware/auth");
const router = express.Router();

router.post("/register", registerUser);
router.post("/login", loginUser);
router.get("/profile", auth, getProfile);

module.exports = router;
```

---

### ✅ `controllers/user.controller.js`

```js
const userService = require("../services/user.service");

exports.registerUser = async (req, res, next) => {
  try {
    const data = await userService.register(req.body);
    res.status(201).json(data);
  } catch (err) {
    next(err); // pass error to global error handler
  }
};

exports.loginUser = async (req, res, next) => {
  try {
    const token = await userService.login(req.body);
    res.json({ token });
  } catch (err) {
    next(err);
  }
};

exports.getProfile = async (req, res, next) => {
  try {
    const user = await userService.getUserById(req.user.id);
    res.json(user);
  } catch (err) {
    next(err);
  }
};
```

---

### ✅ `services/user.service.js`

```js
const User = require("../models/user.model");
const bcrypt = require("bcryptjs");
const { generateToken } = require("../utils/token");

exports.register = async ({ name, email, password }) => {
  const existing = await User.findOne({ email });
  if (existing) throw new Error("User already exists");
  const hashed = await bcrypt.hash(password, 10);
  const user = await User.create({ name, email, password: hashed });
  return { id: user._id, email: user.email };
};

exports.login = async ({ email, password }) => {
  const user = await User.findOne({ email });
  if (!user) throw new Error("Invalid credentials");
  const match = await bcrypt.compare(password, user.password);
  if (!match) throw new Error("Invalid credentials");
  return generateToken(user._id);
};

exports.getUserById = async (id) => {
  return await User.findById(id).select("-password");
};
```

---

### ✅ `utils/token.js`

```js
const jwt = require("jsonwebtoken");

exports.generateToken = (userId) => {
  return jwt.sign({ id: userId }, process.env.JWT_SECRET, {
    expiresIn: "7d",
  });
};
```

---

### ✅ `middleware/auth.js`

```js
const jwt = require("jsonwebtoken");
const User = require("../models/user.model");

module.exports = async (req, res, next) => {
  const token = req.headers.authorization?.split(" ")[1];
  if (!token) return res.status(401).send("No token");
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = await User.findById(decoded.id).select("-password");
    next();
  } catch (err) {
    res.status(401).send("Invalid token");
  }
};
```

---

## ✅ `validators/user.validator.js` (Optional with `express-validator`)

```js
const { check } = require("express-validator");

exports.validateRegister = [
  check("email", "Invalid email").isEmail(),
  check("password", "Minimum 6 chars").isLength({ min: 6 }),
];
```

---

## 📈 **4. How to Extend This**

| Feature          | How                                                |
| ---------------- | -------------------------------------------------- |
| Add Product APIs | `product.routes.js`, `product.controller.js`, etc. |
| Add Unit Tests   | Use `Jest` or `Mocha` in `tests/` folder           |
| Handle Errors    | Create `errorHandler.js` middleware                |
| Add Logging      | Use `winston`, store in `logs/`                    |
| Swagger Docs     | Add OpenAPI using `swagger-jsdoc`                  |
| Role-based auth  | Middleware like `checkRole('admin')`               |

---

## 🧠 **When to Use Node.js + Express.js**

| Use Case                                      | Why Node.js & Express     |
| --------------------------------------------- | ------------------------- |
| Real-time chat app                            | Event-driven, WebSockets  |
| REST APIs                                     | Lightweight, JSON-native  |
| Microservices                                 | Fast, modular             |
| Streaming apps                                | Streams & buffer handling |
| Single Page Applications (with React/Angular) | Full JS stack             |
| Web scraping / CLI tools                      | Scriptable backend        |

---

## 🔗 **Popular Express Middlewares**

| Middleware          | Purpose                       |
| ------------------- | ----------------------------- |
| `cors`              | Handle CORS headers           |
| `morgan`            | HTTP request logging          |
| `helmet`            | Set security headers          |
| `body-parser`       | Parse incoming request bodies |
| `express-validator` | Request validation            |
| `jsonwebtoken`      | JWT authentication            |

---

## 🧪 **Testing Express Apps**

Use tools like:

- **Postman** for manual testing
- **Supertest** + **Jest** or **Mocha** for automated testing

---

## 🚀 **Deployment**

- Use **PM2** to run and monitor production server
- Use **Docker** for containerization
- Deploy on **Vercel**, **Render**, **Heroku**, or **EC2**
- Use **Nginx** as a reverse proxy

---

## 🧾 **Summary Table**

| Feature     | Node.js                | Express.js          |
| ----------- | ---------------------- | ------------------- |
| Role        | Runtime environment    | Web framework       |
| Language    | JavaScript             | JavaScript          |
| Purpose     | Run JS outside browser | Build APIs/web apps |
| Key Concept | Event loop, async I/O  | Middleware, routing |
| Use With    | MongoDB, MySQL, Redis  | Node.js only        |
| Output      | Dynamic data/services  | HTTP responses      |

---

## 💡 Pro Tips

- Always handle **async errors** with `try/catch` or `.catch()`.
- Use **dotenv** for managing secrets.
- Structure your app modularly — don’t keep all logic in one file.
- Combine with **MongoDB** (Mongoose) or **MySQL** (Sequelize) for DB operations.

---

# 📘 Express.js Routing – In-Depth Guide

---

## 🔁 **What is Routing in Express?**

**Routing** defines how your application responds to client requests to specific **URLs** using specific **HTTP methods (GET, POST, etc.)**.

📍 It links:

- a **URL path**
- an **HTTP method**
- and a **handler function (controller)**

---

## ✅ **Basic Routing Example**

```js
app.get("/", (req, res) => {
  res.send("Home Page");
});

app.post("/submit", (req, res) => {
  res.send("Form Submitted");
});
```

---

## 🔗 **Types of Routes**

### 🔹 a) **Static Routes**

These respond to a **fixed path**:

```js
app.get("/about", (req, res) => {
  res.send("About Page");
});
```

---

### 🔹 b) **Dynamic Routes (Route Parameters)**

Use `:` to define **dynamic segments** in the URL.

```js
app.get("/users/:id", (req, res) => {
  const userId = req.params.id; // dynamic part
  res.send(`User ID is ${userId}`);
});
```

✔ You can have multiple parameters:

```js
app.get("/users/:userId/orders/:orderId", (req, res) => {
  res.send(req.params); // { userId: '12', orderId: '99' }
});
```

---

### 🔹 c) **Query Parameters (Optional Filters)**

Accessed via `req.query`:

```js
app.get("/products", (req, res) => {
  const category = req.query.category; // /products?category=mobiles
  res.send(`Filtered by category: ${category}`);
});
```

🧠 Query params are best for:

- search filters
- sorting
- pagination
- optional params

---

### 🔹 d) **Wildcard Routes (Catch-all)**

```js
app.get("*", (req, res) => {
  res.status(404).send("Page Not Found");
});
```

---

## 🚏 **HTTP Methods in Express Routing**

| Method | Purpose             | Example                    |
| ------ | ------------------- | -------------------------- |
| GET    | Read data           | `app.get('/users')`        |
| POST   | Create new data     | `app.post('/users')`       |
| PUT    | Replace data        | `app.put('/users/:id')`    |
| PATCH  | Update part of data | `app.patch('/users/:id')`  |
| DELETE | Remove data         | `app.delete('/users/:id')` |

---

## 🧱 **Modular Routing (Split by Feature)**

### 📁 Structure:

```
routes/
  ├── user.routes.js
  └── product.routes.js
```

### `user.routes.js`:

```js
const express = require("express");
const router = express.Router();
const userController = require("../controllers/user.controller");

router.get("/", userController.getUsers);
router.get("/:id", userController.getUserById);
router.post("/", userController.createUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

module.exports = router;
```

### In `server.js`:

```js
const userRoutes = require("./routes/user.routes");
app.use("/api/users", userRoutes);
```

➡️ Now:

- `GET /api/users`
- `GET /api/users/:id`

---

## 🔐 **Middleware per Route**

Apply middleware (like auth or validation) to individual routes:

```js
router.get("/profile", authMiddleware, userController.getProfile);
```

Or group middleware:

```js
router.use(authMiddleware); // applies to all below
router.get("/profile", userController.getProfile);
```

---

## ⚙️ **Advanced: Route Grouping with Prefix**

Use routers to group routes under a common prefix:

```js
// server.js
app.use("/api/users", require("./routes/user.routes"));
app.use("/api/products", require("./routes/product.routes"));
```

---

## 🧠 **Controller-Based Routing (Clean Architecture)**

👉 **Don't put logic in routes**. Delegate to controllers/services.

```js
router.post("/login", authController.loginUser);
```

### `auth.controller.js`

```js
exports.loginUser = async (req, res) => {
  const { email, password } = req.body;
  const token = await authService.login(email, password);
  res.json({ token });
};
```

---

## 🧪 **Route Validation**

Use `express-validator` or custom validation middleware.

```js
const { body } = require("express-validator");

router.post(
  "/register",
  [body("email").isEmail(), body("password").isLength({ min: 6 })],
  userController.registerUser
);
```

---

## 🧾 **Full Example: Dynamic + Query Route**

```js
// GET /api/users/123?showDetails=true

app.get("/api/users/:id", (req, res) => {
  const userId = req.params.id;
  const showDetails = req.query.showDetails === "true";
  if (showDetails) {
    res.send(`Details for user ${userId}`);
  } else {
    res.send(`Basic info for user ${userId}`);
  }
});
```

---

## 💡 Best Practices

| Tip                             | Why                             |
| ------------------------------- | ------------------------------- |
| Use `Router()` for each feature | Clean modular code              |
| Keep routes thin                | Delegate to controller/services |
| Prefix API routes with `/api`   | Consistent API paths            |
| Validate input in routes        | Prevent bad data                |
| Handle errors centrally         | Use custom error middleware     |
| Avoid `app.get('*')` at the top | It will override other routes   |

---

## ✅ Summary

| Type            | Usage                        | Syntax              |
| --------------- | ---------------------------- | ------------------- |
| Static          | Fixed paths                  | `/about`            |
| Dynamic         | `:param`                     | `/users/:id`        |
| Query           | After `?`                    | `/users?role=admin` |
| Wildcard        | `*`                          | `app.get('*')`      |
| Nested          | `/users/:id/orders/:orderId` | Yes                 |
| Grouped/Modular | Separate files + `Router()`  | Yes                 |

---

## 🧠 What Are `request` and `response` objects in Express.js?

In Express, when a client (like a React frontend) makes an HTTP request:

- `req` (short for `request`) is the object that contains **everything sent from the client**.
- `res` (short for `response`) is what **you use to send data back** to the client.

```js
app.get("/hello", (req, res) => {
  res.send("Hi from server");
});
```

---

## 🔹 `req` (Request Object) — In Depth

`req` contains **everything the client sends**.

### 1. `req.method`

HTTP method: `"GET"`, `"POST"`, `"PUT"`, `"DELETE"`, etc.

```js
console.log(req.method); // "POST"
```

---

### 2. `req.url` and `req.originalUrl`

- `req.url`: Path after domain
- `req.originalUrl`: Includes middleware routing

```js
// GET /users/123
req.url; // "/123"
req.originalUrl; // "/users/123"
```

---

### 3. `req.params`

Route parameters (`:id`, `:slug`, etc.)

```js
app.get("/user/:id", (req, res) => {
  console.log(req.params.id); // /user/5 → 5
});
```

---

### 4. `req.query`

Query string params after `?`

```js
// /search?term=react&page=2
req.query.term; // "react"
req.query.page; // "2"
```

---

### 5. `req.body`

POST/PUT request data — you must use a body parser like:

```js
app.use(express.json()); // To read JSON body

// POST { "name": "Shubham" }
console.log(req.body.name);
```

---

### 6. `req.headers`

All HTTP headers sent by the client.

```js
console.log(req.headers["authorization"]);
```

---

### 7. `req.cookies`

If using `cookie-parser` middleware.

```js
// after app.use(cookieParser())
console.log(req.cookies.token); // Read cookies
```

---

### 8. `req.ip`

Client's IP address.

---

### 9. `req.get(headerName)`

Shortcut to read a header:

```js
req.get("User-Agent"); // Chrome, Firefox etc.
```

---

## 🔸 `res` (Response Object) — In Depth

Use `res` to **respond to the client**.

### 1. `res.send()`

Send response text, JSON, object, etc.

```js
res.send("Hello");
res.send({ msg: "Success" });
```

---

### 2. `res.json()`

Specifically for JSON responses.

```js
res.json({ status: "ok", user: "Shubham" });
```

---

### 3. `res.status(code)`

Set HTTP status code.

```js
res.status(404).send("Not Found");
```

---

### 4. `res.set(headerName, value)`

Set custom response headers.

```js
res.set("X-Powered-By", "ShubhamAPI");
```

---

### 5. `res.redirect(url)`

Redirect client to another URL.

```js
res.redirect("/login");
```

---

### 6. `res.sendFile()`

Send a file to the client.

```js
res.sendFile(__dirname + "/index.html");
```

---

### 7. `res.cookie(name, value)`

Set a cookie (with `cookie-parser` or `res.cookie()`):

```js
res.cookie("token", "123abc", {
  httpOnly: true,
  secure: true,
  maxAge: 1000 * 60 * 60, // 1 hour
});
```

---

### 8. `res.clearCookie(name)`

Remove cookie:

```js
res.clearCookie("token");
```

---

### 9. `res.end()`

Ends the response process without sending data.

---

## 🔄 Full Example

```js
app.post("/login", (req, res) => {
  const { username, password } = req.body;

  // Do auth logic...

  // Set token as cookie
  res.cookie("token", "abc123", { httpOnly: true });

  // Send response
  res.status(200).json({
    message: "Login success",
    user: username,
  });
});
```

---

## ⚠️ Middleware and Chaining

Both `req` and `res` are passed through **middleware** in Express.

```js
app.use((req, res, next) => {
  console.log("Incoming Request:", req.method, req.url);
  next(); // Go to next middleware/route
});
```

---

## ✅ Summary Table

| Feature       | `req`                      | `res`                               |
| ------------- | -------------------------- | ----------------------------------- |
| Body Data     | `req.body`                 |                                     |
| Headers       | `req.headers`, `req.get()` | `res.set()`                         |
| Params/Query  | `req.params`, `req.query`  |                                     |
| Cookie        | `req.cookies`              | `res.cookie()`, `res.clearCookie()` |
| Status        |                            | `res.status(code)`                  |
| Response Data |                            | `res.send()`, `res.json()`          |
| Files         |                            | `res.sendFile()`                    |
| Redirect      |                            | `res.redirect()`                    |

---

# 🧠 What is Middleware in Express.js?

---

### ✅ **Definition:**

> Middleware functions are functions that have access to the **request (req)**, **response (res)**, and **next** function in the request-response cycle.

```js
(req, res, next) => {
  /* logic */
};
```

They can:

- Modify `req` or `res`
- End the request-response cycle
- Pass control to the next middleware (`next()`)

---

## 🔁 Middleware Lifecycle:

```txt
Incoming Request → Middleware 1 → Middleware 2 → Route Handler → Response
```

---

## 🧩 Why Use Middleware?

- ✅ Separation of concerns
- ✅ Reusability (e.g. auth, logging)
- ✅ Centralized logic (e.g. error handling)
- ✅ Cleaner code in route handlers

---

# 📚 Types of Middleware in Express.js

---

## 🔹 1. **Application-Level Middleware**

Used across the entire app using `app.use()` or `app.METHOD()`.

### ✅ Example: Logging Middleware

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});
```

📌 Runs for every request and logs method + URL.

---

## 🔹 2. **Router-Level Middleware**

Attached only to a specific `Router()`.

```js
const router = express.Router();

router.use((req, res, next) => {
  console.log("Middleware for user routes only");
  next();
});
```

✅ Only runs for routes defined in that router.

---

## 🔹 3. **Built-in Middleware**

### `express.json()`

Parses incoming JSON:

```js
app.use(express.json());
```

### `express.urlencoded()`

Parses form-encoded data:

```js
app.use(express.urlencoded({ extended: true }));
```

---

## 🔹 4. **Third-Party Middleware**

Middleware installed via NPM.

### Example: `cors` – Enable cross-origin requests

```bash
npm install cors
```

```js
const cors = require("cors");
app.use(cors());
```

### Example: `morgan` – HTTP request logger

```bash
npm install morgan
```

```js
const morgan = require("morgan");
app.use(morgan("dev"));
```

---

## 🔹 5. **Error-Handling Middleware**

Special signature: **`(err, req, res, next)`**

### ✅ Example:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

📌 This should always be **last** in the middleware chain.

---

## 🔹 6. **Custom Middleware (Business Logic)**

### Example: Authentication Middleware

```js
const auth = (req, res, next) => {
  const token = req.headers.authorization;
  if (token === "12345") next();
  else res.status(401).send("Unauthorized");
};

app.get("/dashboard", auth, (req, res) => {
  res.send("Secret dashboard");
});
```

---

## 🔹 7. **Conditional Middleware**

Apply only on specific routes.

```js
app.get("/admin", checkAdmin, adminController);
```

Or inline:

```js
app.get("/profile", authMiddleware, (req, res) => {
  res.send("Profile data");
});
```

---

# 🌍 Real-World Use Cases

| Use Case          | Middleware Type      | Real Example              |
| ----------------- | -------------------- | ------------------------- |
| Logging requests  | Application-level    | `morgan`, custom log      |
| Authenticate user | Custom / route-level | `authMiddleware`          |
| Handle CORS       | 3rd-party            | `cors`                    |
| Parse body        | Built-in             | `express.json()`          |
| Rate limiting     | 3rd-party            | `express-rate-limit`      |
| Validate input    | 3rd-party            | `express-validator`       |
| Error handling    | Custom               | `errorHandler` middleware |
| Role-based access | Custom               | `checkRole('admin')`      |

---

# 🧠 Middleware Order Matters

Middleware is executed **in the order you define it**.

```js
app.use(middleware1);
app.use(middleware2);
app.get("/route", handler);
```

If you place `authMiddleware` **after** the route, it will not run for that route.

---

# 🔁 Reusable Middleware Pattern

```js
// middleware/logger.js
module.exports = (req, res, next) => {
  console.log(`[${new Date()}] ${req.method} ${req.url}`);
  next();
};

// In server.js
const logger = require("./middleware/logger");
app.use(logger);
```

---

# ✅ Summary Table

| Middleware Type | Purpose                      | Method                  |
| --------------- | ---------------------------- | ----------------------- |
| App-level       | Applies to all routes        | `app.use()`             |
| Router-level    | Applies to specific router   | `router.use()`          |
| Built-in        | JSON, URL parsing            | `express.json()`        |
| 3rd-party       | Logging, CORS, Rate Limiting | `cors`, `morgan`, etc.  |
| Error-handling  | Catches app-wide errors      | `(err, req, res, next)` |
| Custom          | Auth, Roles, Logger, etc.    | Your logic              |

---

# 🛠️ How to Modify `req` and `res` in Middleware

---

## ✅ Middleware Signature

```js
(req, res, next) => {
  // logic here
  next(); // call next middleware or route handler
};
```

---

## 🔄 What You Can Modify

| Object           | Common Modifications                      |
| ---------------- | ----------------------------------------- |
| `req` (request)  | Add user info, metadata, parsed data      |
| `res` (response) | Set headers, custom response body, status |

---

## 🔵 1. **Modify `req` Object in Middleware**

Add custom properties to the `req` object so they’re accessible in later middleware/route handlers.

### ✅ Example: Add current timestamp to `req`

```js
const addTimestamp = (req, res, next) => {
  req.requestTime = new Date();
  next();
};

app.use(addTimestamp);

app.get("/", (req, res) => {
  res.send(`Request received at ${req.requestTime}`);
});
```

---

### ✅ Example: Attach authenticated user info to `req`

```js
const mockAuth = (req, res, next) => {
  req.user = {
    id: "abc123",
    name: "Shubham",
    role: "admin",
  };
  next();
};

app.use(mockAuth);

app.get("/profile", (req, res) => {
  res.json(req.user); // Access injected user info
});
```

---

## 🔴 2. **Modify `res` Object in Middleware**

### ✅ Set response headers:

```js
const addCustomHeader = (req, res, next) => {
  res.setHeader("X-Powered-By", "ShubhamExpressApp");
  next();
};

app.use(addCustomHeader);
```

---

### ✅ Pre-send custom response:

```js
const blockBlacklistedIP = (req, res, next) => {
  const blacklist = ["192.168.1.100"];
  if (blacklist.includes(req.ip)) {
    return res.status(403).send("Access Denied");
  }
  next();
};

app.use(blockBlacklistedIP);
```

---

### ✅ Override or intercept response

You can even completely change the response before it hits the client.

```js
const customResponse = (req, res, next) => {
  res.send = ((originalSend) => (body) => {
    body = `🔒 Secured: ${body}`;
    return originalSend.call(res, body);
  })(res.send);
  next();
};

app.use(customResponse);

app.get("/", (req, res) => {
  res.send("This is a normal response"); // gets modified!
});
```

---

## 🧠 Advanced Example: Injecting User Role and Logging Response Time

```js
app.use((req, res, next) => {
  req.user = { id: "u1", role: "admin" }; // modify req
  res.setHeader("X-App-Version", "1.0.0"); // modify res

  const start = Date.now();
  res.on("finish", () => {
    const duration = Date.now() - start;
    console.log(`Handled ${req.method} ${req.url} in ${duration}ms`);
  });

  next();
});
```

---

## 📌 Important Notes

- Always call `next()` unless you end the response (`res.send/res.json/res.end`).
- Avoid overriding built-in props like `req.body` unless you're sure.
- Don't modify `res.statusCode` unless needed — it affects client response codes.

---

## ✅ Summary

| Action               | Middleware Example                |
| -------------------- | --------------------------------- |
| Add data to request  | `req.user = {...}`                |
| Set custom header    | `res.setHeader()`                 |
| End response early   | `res.status(403).send('Blocked')` |
| Modify response body | Monkey patch `res.send`           |
| Track performance    | Use `res.on('finish', ...)`       |

---

## 🧾 What is Templating in Express?

**Templating** allows you to create **HTML pages dynamically on the server**, injecting values (like user name, product list, etc.) before sending the final HTML to the client.

Instead of hardcoding HTML, you use **template engines** like:

- ✅ **EJS (Embedded JavaScript)**
- ✅ **Pug (formerly Jade)**
- ✅ **Handlebars**
- ✅ **Nunjucks**

---

## 💡 Why Use Templating?

| Benefit                  | Description                                                 |
| ------------------------ | ----------------------------------------------------------- |
| 📦 Dynamic Content       | Inject data into HTML (e.g., user names, lists, conditions) |
| ♻️ Reusability           | Use partials (e.g., header, footer) for consistent layout   |
| 📄 SEO Friendly          | Server-side rendered HTML (not JS) is easily indexed        |
| 📊 Useful for Dashboards | Admin panels, reports, emails, etc.                         |

---

## 🛠️ How to Use a Templating Engine in Express (EJS Example)

### 🔹 Step 1: Install the engine

```bash
npm install ejs
```

---

### 🔹 Step 2: Set up view engine in Express

```js
const express = require("express");
const app = express();

app.set("view engine", "ejs"); // set EJS as the templating engine
app.set("views", "./views"); // optional, default is "./views"
```

---

### 🔹 Step 3: Create an `.ejs` file inside `/views`

📁 `views/home.ejs`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Welcome</title>
  </head>
  <body>
    <h1>Hello, <%= user %>!</h1>
    <ul>
      <% items.forEach(item => { %>
      <li><%= item %></li>
      <% }) %>
    </ul>
  </body>
</html>
```

---

### 🔹 Step 4: Render it in a route

```js
app.get("/", (req, res) => {
  res.render("home", {
    user: "Shubham",
    items: ["React", "Node.js", "MongoDB"],
  });
});
```

---

## 🧩 Other Modern Template Engines

| Engine               | Syntax Style         | Pros                             | Install                |
| -------------------- | -------------------- | -------------------------------- | ---------------------- |
| **EJS**              | HTML + `<%= %>`      | Simple, HTML-like                | `npm install ejs`      |
| **Pug**              | Indentation-based    | Short syntax                     | `npm install pug`      |
| **Handlebars (hbs)** | `{{name}}`           | Popular for logic-less templates | `npm install hbs`      |
| **Nunjucks**         | Jinja2-like (Python) | Powerful & readable              | `npm install nunjucks` |

---

## 🧪 Example: Pug Engine

```bash
npm install pug
```

```js
app.set("view engine", "pug");
```

📁 `views/profile.pug`

```pug
html
  head
    title Profile
  body
    h1 Hello #{user}
    ul
      each item in items
        li= item
```

---

## 🧠 When to Use Templating (in Modern Apps)

- 🖥️ Server-rendered web pages (admin panels, marketing pages)
- ✉️ Dynamic HTML emails
- 🧪 Prototypes with backend-rendered HTML
- 📊 Dashboards or CMS-style tools

---

## ⚠️ Note: SPA vs SSR

If you're using **React/Next.js**, templating is usually done **on the frontend**, but Express templating is still valuable for:

- Admin interfaces
- Email generation
- SEO pre-rendered content
- Lightweight apps without frontend frameworks

---

## ✅ Summary

| Feature        | Express Templating              |
| -------------- | ------------------------------- |
| Setup          | `app.set('view engine', 'ejs')` |
| Render         | `res.render('file', data)`      |
| Data Injection | `<%= user %>`, `{{user}}`, etc. |
| Partial Views  | Can include headers, footers    |
| Use Cases      | SSR pages, emails, dashboards   |

---

## 🧾 What is Static File Serving?

In Express.js, **static files** refer to files like:

- `HTML`, `CSS`, `JS`
- Images (`.jpg`, `.png`)
- Fonts, videos, audio
- PDFs or other client-deliverable files

> **Static file serving** means letting the Express server send these files directly to the browser without processing.

---

## 🛠️ How to Serve Static Files in Express

### 🔹 Step 1: Create a `public` folder

```
project/
├── public/
│   ├── index.html
│   ├── styles.css
│   └── script.js
└── app.js
```

---

### 🔹 Step 2: Use `express.static` middleware

```js
const express = require("express");
const app = express();
const path = require("path");

// Serve static files from 'public' folder
app.use(express.static(path.join(__dirname, "public")));

app.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

Now, you can access:

- `http://localhost:3000/index.html`
- `http://localhost:3000/styles.css`
- `http://localhost:3000/script.js`

---

## 📂 Advanced: Custom URL path for static files

```js
app.use("/static", express.static(path.join(__dirname, "public")));
```

Now access via:

- `http://localhost:3000/static/index.html`

---

## 📦 Real-World Use Case

### Example: Serving React Build

If you built a React app (`npm run build`), you can serve it with Express:

```js
app.use(express.static(path.join(__dirname, "client/build")));

app.get("*", (req, res) => {
  res.sendFile(path.join(__dirname, "client/build", "index.html"));
});
```

---

## 🔐 Best Practices

| Practice                                | Benefit                                            |
| --------------------------------------- | -------------------------------------------------- |
| ✅ Use `path.join`                      | Ensures cross-platform path compatibility          |
| ✅ Set Cache-Control headers (optional) | Improve performance with long-lived caching        |
| ❌ Don't serve sensitive files          | Keep config or `.env` files outside static folders |

---

## 🧪 Example: Serving Profile Images

```js
// /uploads/profile.jpg
app.use("/uploads", express.static(path.join(__dirname, "uploads")));

// Access via: http://localhost:3000/uploads/profile.jpg
```

---

## ✅ Summary

| Concept      | Description                                     |
| ------------ | ----------------------------------------------- |
| Middleware   | `express.static()`                              |
| Default path | `public` folder                                 |
| Use cases    | CSS, JS, images, favicon, fonts                 |
| Real use     | Frontend apps, public files, uploads, SEO pages |

---

## 🧾 What is URL-encoded Data?

When an HTML `<form>` is submitted using the default `method="POST"` and `enctype="application/x-www-form-urlencoded"`, the data is encoded in the URL-style format:

```
name=Shubham&email=shubham%40example.com
```

This is what we call **URL-encoded data**.

---

## 🧰 Step-by-Step: Handling Form Data in Express

### ✅ 1. Create a Simple HTML Form

```html
<!-- public/form.html -->
<form action="/submit" method="POST">
  <input type="text" name="username" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</form>
```

---

### ✅ 2. Middleware to Parse URL-encoded Data

In your `app.js`:

```js
const express = require("express");
const path = require("path");
const app = express();

// Required to parse form data (application/x-www-form-urlencoded)
app.use(express.urlencoded({ extended: true }));

// To serve HTML form
app.use(express.static(path.join(__dirname, "public")));

// Handle form submission
app.post("/submit", (req, res) => {
  console.log(req.body); // { username: '...', email: '...' }

  res.send(`Received: ${req.body.username} - ${req.body.email}`);
});

app.listen(3000, () => console.log("Server running on http://localhost:3000"));
```

---

## ⚙️ `extended: true` vs `false`

| Option  | Behavior                                                |
| ------- | ------------------------------------------------------- |
| `true`  | Uses the `qs` library (supports nested objects, arrays) |
| `false` | Uses the `querystring` module (simpler, flat data only) |

Example with nested fields:

```html
<input type="text" name="user[name]" /> <input type="text" name="user[email]" />
```

```js
// extended: true ➝ req.body = { user: { name: '...', email: '...' } }
// extended: false ➝ req.body = { 'user[name]': '...', 'user[email]': '...' }
```

---

## ✅ Also Handling JSON? Use Both Parsers

```js
app.use(express.json()); // For JSON requests
app.use(express.urlencoded({ extended: true })); // For form submissions
```

---

## 💡 Real-World Use Case: Login Form

```html
<form action="/login" method="POST">
  <input name="email" />
  <input name="password" type="password" />
</form>
```

```js
app.post("/login", (req, res) => {
  const { email, password } = req.body;
  // Validate and authenticate user
});
```

---

## 🔐 Security Tip

Always sanitize and validate form input before processing or storing to prevent:

- XSS
- SQL Injection
- Command Injection

Use libraries like:

- `express-validator`
- `sanitize-html`
- `helmet` for setting secure HTTP headers

---

## ✅ Summary

| Task                      | Solution                                             |
| ------------------------- | ---------------------------------------------------- |
| Parse HTML `<form>` data  | `express.urlencoded({ extended: true })`             |
| Handle nested form fields | Use `extended: true`                                 |
| Handle both form and JSON | Use both `express.urlencoded()` and `express.json()` |
| Serve the form            | Use `express.static()` or a templating engine        |

---

## 📦 1. Install Required Packages

```bash
npm install express multer
```

---

## 📁 Folder Structure

```
project/
├── uploads/             ← where files will be stored
├── public/              ← static folder for HTML forms
│   └── form.html
├── app.js
```

---

## 🧱 Setup Multer in `app.js`

```js
const express = require("express");
const multer = require("multer");
const path = require("path");

const app = express();

// Setup storage
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, "uploads/"); // save to uploads folder
  },
  filename: function (req, file, cb) {
    const uniqueSuffix = Date.now() + "-" + Math.round(Math.random() * 1e9);
    const ext = path.extname(file.originalname);
    cb(null, file.fieldname + "-" + uniqueSuffix + ext);
  },
});

const upload = multer({ storage });

app.use(express.urlencoded({ extended: true }));
app.use(express.json());
app.use(express.static("public"));
```

---

# ✅ 1. Text Fields Only

### **HTML (`form.html`)**

```html
<form action="/text-only" method="POST">
  <input type="text" name="username" />
  <input type="email" name="email" />
  <button type="submit">Submit</button>
</form>
```

### **Express Route**

```js
app.post("/text-only", (req, res) => {
  console.log(req.body); // { username: '...', email: '...' }
  res.send("Text data received");
});
```

---

# ✅ 2. Text Fields + Single File

### **HTML**

```html
<form action="/single-upload" method="POST" enctype="multipart/form-data">
  <input type="text" name="username" />
  <input type="file" name="profile" />
  <button type="submit">Upload</button>
</form>
```

### **Route**

```js
app.post("/single-upload", upload.single("profile"), (req, res) => {
  console.log(req.body); // { username: '...' }
  console.log(req.file); // file info
  res.send("Single file uploaded");
});
```

---

# ✅ 3. Text Fields + Multiple Files (Same Field Name)

### **HTML**

```html
<form action="/multi-upload" method="POST" enctype="multipart/form-data">
  <input type="text" name="title" />
  <input type="file" name="photos" multiple />
  <button type="submit">Upload</button>
</form>
```

### **Route**

```js
app.post("/multi-upload", upload.array("photos", 5), (req, res) => {
  console.log(req.body); // { title: '...' }
  console.log(req.files); // array of files
  res.send("Multiple files uploaded");
});
```

---

# ✅ 4. Text Fields + Multiple Files (Different Field Names)

### **HTML**

```html
<form action="/multi-fields" method="POST" enctype="multipart/form-data">
  <input type="text" name="username" />
  <input type="file" name="avatar" />
  <input type="file" name="resume" />
  <button type="submit">Upload</button>
</form>
```

### **Route**

```js
app.post(
  "/multi-fields",
  upload.fields([
    { name: "avatar", maxCount: 1 },
    { name: "resume", maxCount: 1 },
  ]),
  (req, res) => {
    console.log(req.body); // { username: '...' }
    console.log(req.files); // { avatar: [...], resume: [...] }
    res.send("Multiple fields uploaded");
  }
);
```

---

## 📂 Uploaded Files Structure

```js
// Single file ➝ req.file = { originalname, filename, path, mimetype, size }
// Multiple files ➝ req.files = [ { ... }, { ... } ]
// .fields() ➝ req.files = { avatar: [...], resume: [...] }
```

---

## 🧼 Bonus: File Type & Size Filtering

```js
const upload = multer({
  storage,
  fileFilter: function (req, file, cb) {
    const allowed = [".jpg", ".jpeg", ".png", ".pdf"];
    const ext = path.extname(file.originalname).toLowerCase();
    if (!allowed.includes(ext)) {
      return cb(new Error("Only images and PDFs allowed"));
    }
    cb(null, true);
  },
  limits: { fileSize: 5 * 1024 * 1024 }, // 5MB
});
```

---

## ✅ Summary Table

| Use Case                    | Middleware                 |
| --------------------------- | -------------------------- |
| Text fields only            | `express.urlencoded()`     |
| Text + Single file          | `upload.single('field')`   |
| Text + Multiple (same name) | `upload.array('field', n)` |
| Text + Multiple (diff name) | `upload.fields([...])`     |

---

## 🔷 What is Mongoose?

**Mongoose** is an **Object Data Modeling (ODM)** library for **MongoDB** and **Node.js**. It provides:

- Schema-based solution to model application data
- Built-in type casting, validation, query building, business logic hooks, and more

---

## 📦 Installation

```bash
npm install mongoose
```

---

## 🧩 Mongoose: Connecting to MongoDB

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB Connected");
  } catch (err) {
    console.error(err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

---

## 🧱 Defining a Schema

Schemas define the **structure and rules** for documents.

```js
const { Schema } = require("mongoose");

const userSchema = new Schema({
  name: {
    type: String,
    required: [true, "Name is required"], // Custom error message
    minlength: [3, "Name must be at least 3 characters"],
    maxlength: [50, "Name must be at most 50 characters"],
    trim: true, // Removes leading/trailing whitespaces
  },
  email: {
    type: String,
    required: [true, "Email is required"],
    unique: true, // Creates a unique index
    lowercase: true, // Automatically converts to lowercase
    match: [
      /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/,
      "Please fill a valid email address",
    ],
  },
  age: {
    type: Number,
    min: [18, "Minimum age is 18"],
    max: [65, "Maximum age is 65"],
    validate: {
      validator: Number.isInteger,
      message: "Age must be an integer",
    },
  },
  isAdmin: {
    type: Boolean,
    default: false,
  },
  role: {
    type: String,
    enum: {
      values: ["user", "admin", "moderator"],
      message: "{VALUE} is not a valid role",
    },
    default: "user",
  },
  phone: {
    type: String,
    validate: {
      validator: function (v) {
        return /^(\+91)?[6-9]\d{9}$/.test(v);
      },
      message: (props) => `${props.value} is not a valid Indian phone number`,
    },
    required: [true, "Phone number is required"],
  },
  password: {
    type: String,
    required: [true, "Password is required"],
    minlength: [6, "Password must be at least 6 characters long"],
    select: false, // Will not be selected by default in queries
  },
  confirmPassword: {
    type: String,
    required: true,
    validate: {
      validator: function (val) {
        // `this` only works on `save` and `create`, not update
        return val === this.password;
      },
      message: "Passwords do not match",
    },
  },
  createdAt: {
    type: Date,
    default: Date.now,
    immutable: true, // Field cannot be changed once set
  },
});
```

---

## ✅ Summary of Validations Used

| Validation               | Field(s)                          | Description                       |
| ------------------------ | --------------------------------- | --------------------------------- |
| `required`               | All major fields                  | Ensures value is provided         |
| `minlength`, `maxlength` | `name`, `password`                | Restricts string length           |
| `min`, `max`             | `age`                             | Restricts number range            |
| `match`                  | `email`                           | Validates against regex           |
| `enum`                   | `role`                            | Restricts to allowed values       |
| `validate` (custom)      | `age`, `phone`, `confirmPassword` | Custom logic                      |
| `lowercase`              | `email`                           | Auto-lowercases                   |
| `trim`                   | `name`                            | Removes extra space               |
| `select: false`          | `password`                        | Hides field from query results    |
| `immutable`              | `createdAt`                       | Locks the value on creation       |
| `unique`                 | `email`                           | Creates a unique index in MongoDB |

---

## 🏗️ Creating a Model

A model is a **constructor compiled from a schema**.

```js
const User = mongoose.model("User", userSchema);
```

- `User` is the model name (auto-pluralized to `users` in MongoDB)
- Used to perform CRUD operations

---

## 🟢 Insert (Create) Documents in Depth

### ✅ 1. Using `.create()`

```js
const newUser = await User.create({
  name: "Shubham",
  email: "shubham@example.com",
  age: 27,
});
```

- Returns a promise
- Automatically runs validations and middleware

### ✅ 2. Using `.save()` on a document instance

```js
const user = new User({
  name: "Shubham",
  email: "shubham@example.com",
  age: 27,
});

await user.save();
```

- Gives more control
- Allows mutation before saving

---

## 🛠️ Advanced Create Use Cases

### ➤ Validation Errors

```js
try {
  await User.create({ name: "Missing Email" });
} catch (err) {
  console.log(err.errors.email.message); // Required validation error
}
```

### ➤ Inserting Multiple Documents

```js
await User.insertMany([
  { name: "User1", email: "u1@example.com" },
  { name: "User2", email: "u2@example.com" },
]);
```

> `insertMany` is faster and skips some middleware unless explicitly enabled.

---

## 🎯 Middleware (Hooks) for Create

```js
userSchema.pre("save", function (next) {
  console.log("Before saving:", this.name);
  next();
});

userSchema.post("save", function (doc, next) {
  console.log("Saved:", doc.name);
  next();
});
```

---

Perfect! Here's **Part 2** of the complete in-depth Mongoose guide, including **Read, Update, Delete**, and more. Best practices and real-world patterns are highlighted throughout.

---

## 📖 READ: Fetching Data

### ✅ 1. `find()`: Fetch multiple documents

```js
const users = await User.find({ isAdmin: false });
```

- Returns an array
- Pass empty `{}` to fetch all

---

### ✅ 2. `findOne()`: Fetch single document

```js
const user = await User.findOne({ email: "shubham@example.com" });
```

- Returns the first match or `null`

---

### ✅ 3. `findById()`: Find by `_id`

```js
const user = await User.findById("64d4cabc123abc123abc1234");
```

- `_id` is a unique ObjectId

---

### 🔍 Projection (Include/Exclude Fields)

```js
User.find({}, "name email"); // include name, email only
User.find({}, { password: 0 }); // exclude password
```

---

### 🔎 Filtering with Query Operators

```js
User.find({ age: { $gt: 25, $lte: 40 } });
```

Operators like `$gt`, `$gte`, `$lt`, `$in`, `$or`, `$regex`, etc.

---

## 🧑‍💻 Real-World Pattern: Pagination

```js
const page = 1;
const limit = 10;

const users = await User.find()
  .skip((page - 1) * limit)
  .limit(limit);
```

---

## 🔄 UPDATE

### ✅ 1. `updateOne()`

```js
await User.updateOne({ email: "shubham@example.com" }, { age: 30 });
```

---

### ✅ 2. `updateMany()`

```js
await User.updateMany({ isAdmin: false }, { isAdmin: true });
```

---

### ✅ 3. `findByIdAndUpdate()`

```js
await User.findByIdAndUpdate(
  "64d4cabc123abc123abc1234",
  { name: "Updated Name" },
  { new: true, runValidators: true }
);
```

> `new: true` → returns updated doc
> `runValidators: true` → applies schema validation on update

---

## ❌ DELETE

### ✅ 1. `deleteOne()`

```js
await User.deleteOne({ email: "shubham@example.com" });
```

---

### ✅ 2. `deleteMany()`

```js
await User.deleteMany({ isAdmin: false });
```

---

### ✅ 3. `findByIdAndDelete()`

```js
await User.findByIdAndDelete("64d4cabc123abc123abc1234");
```

---

## 📈 INDEXING

### ✅ Declaring Index

```js
const productSchema = new mongoose.Schema({
  name: String,
  category: String,
  price: Number,
});

productSchema.index({ category: 1 }); // ascending index
```

### ✅ Compound Index

```js
productSchema.index({ category: 1, price: -1 });
```

> Improves query speed. Always match index order in queries.

---

## 🧪 VIRTUALS

Used to define **computed fields** not stored in DB.

```js
userSchema.virtual("fullName").get(function () {
  return this.name + " (Admin: " + this.isAdmin + ")";
});

const user = await User.findOne();
console.log(user.fullName); // Virtual field
```

---

## 🧩 POPULATE (Relations)

```js
// Post schema
const postSchema = new mongoose.Schema({
  title: String,
  content: String,
  author: { type: mongoose.Schema.Types.ObjectId, ref: "User" },
});

// Get post with author data
const posts = await Post.find().populate("author", "name email");
```

---

## 📌 Best Practices

| Practice                                                         | Why                                           |
| ---------------------------------------------------------------- | --------------------------------------------- |
| Always validate input                                            | Prevent malformed data                        |
| Use `lean()` for read-only queries                               | Returns plain JS object, improves performance |
| Use pagination with `.skip()` and `.limit()`                     | Handles large datasets                        |
| Avoid `.findOne().then(...).catch(...)` mix with `async/await`   | Stick to one async style                      |
| Index fields used in search/sort                                 | Faster reads                                  |
| Avoid storing passwords in plain text                            | Always hash them                              |
| Use `.toJSON()` or `.toObject()` with `transform` to hide fields | Remove private info from response             |
| Separate Mongoose connection logic from route logic              | Better modularity                             |
| Use `.create()` for one or many inserts                          | Auto-handles validation                       |
| Use Mongoose transactions for multi-step operations              | Ensures atomicity                             |

---

## 🚀 Real-World Example: Auth User Model

```js
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
    match: /^\S+@\S+\.\S+$/,
  },
  password: { type: String, required: true, minlength: 6 },
});

// Hash before saving
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();
  this.password = await bcrypt.hash(this.password, 10);
  next();
});

// Method to check password
userSchema.methods.comparePassword = async function (candidate) {
  return bcrypt.compare(candidate, this.password);
};

module.exports = mongoose.model("User", userSchema);
```

---

## 🔚 Summary

| Topic          | Methods                                              |
| -------------- | ---------------------------------------------------- |
| Connect        | `mongoose.connect()`                                 |
| Create         | `.create()`, `.save()`, `insertMany()`               |
| Read           | `.find()`, `.findOne()`, `.findById()`               |
| Update         | `.updateOne()`, `.findByIdAndUpdate()`               |
| Delete         | `.deleteOne()`, `.findByIdAndDelete()`               |
| Indexing       | `schema.index()`                                     |
| Virtuals       | `schema.virtual()`                                   |
| Relations      | `populate()`                                         |
| Best Practices | Indexing, validations, pagination, modular structure |

---

# 🔄 1. Aggregation Pipelines in Mongoose (Advanced MongoDB Queries)

### ✅ What is Aggregation?

Aggregation is used to **process data records** and **return computed results**, like SQL `GROUP BY`, filters, projections, etc.

---

### 🔧 Basic Syntax

```js
Model.aggregate([
  { $match: { isAdmin: true } }, // Filter
  { $group: { _id: "$role", count: { $sum: 1 } } }, // Grouping
  { $sort: { count: -1 } }, // Sort descending
]);
```

---

### 🧠 Real-World Example: Group Users by Role

```js
const stats = await User.aggregate([
  { $group: { _id: "$role", totalUsers: { $sum: 1 } } },
]);
```

---

### 🔍 Common Operators

| Stage             | Description                      |
| ----------------- | -------------------------------- |
| `$match`          | Filter documents                 |
| `$group`          | Group by fields and aggregate    |
| `$sort`           | Sort results                     |
| `$project`        | Shape the output (fields/values) |
| `$lookup`         | Perform joins                    |
| `$unwind`         | Deconstruct arrays               |
| `$limit`, `$skip` | Pagination                       |

---

### 🔄 Lookup (Join Example)

```js
Post.aggregate([
  {
    $lookup: {
      from: "users", // Target collection
      localField: "author", // Field in Post
      foreignField: "_id", // Field in User
      as: "authorDetails",
    },
  },
  { $unwind: "$authorDetails" }, // optional if you want flat object
]);
```

---

# 💳 2. Mongoose Transactions

### ✅ What is a Transaction?

A **transaction** ensures multiple operations happen together or not at all — like transferring money between accounts.

---

### 🔐 Setup: MongoDB must be running as a **replica set** (even if just one node):

```bash
mongod --replSet rs0 --port 27017
```

Then initiate:

```bash
rs.initiate()
```

---

### 🧪 Transaction Example (Money Transfer)

```js
const session = await mongoose.startSession();

try {
  session.startTransaction();

  const sender = await User.findById(senderId).session(session);
  const receiver = await User.findById(receiverId).session(session);

  if (sender.balance < amount) throw new Error("Insufficient funds");

  sender.balance -= amount;
  receiver.balance += amount;

  await sender.save({ session });
  await receiver.save({ session });

  await session.commitTransaction();
  console.log("✅ Transaction committed");
} catch (err) {
  await session.abortTransaction();
  console.error("❌ Transaction aborted:", err.message);
} finally {
  session.endSession();
}
```

---

### ✅ Best Practices

- Always `commitTransaction()` or `abortTransaction()`
- Use sessions in all reads/writes
- Catch errors and log them

---

# 🧩 3. Mongoose Plugins

Plugins let you **re-use logic across schemas**. Like mixins for your models.

---

### 🔧 Example: `timestamps` plugin (built-in)

```js
const userSchema = new mongoose.Schema({ name: String }, { timestamps: true });
```

Creates:

```js
{
  createdAt: Date,
  updatedAt: Date
}
```

---

### 🧠 Custom Plugin Example (mask email)

```js
function maskEmailPlugin(schema, options) {
  schema.methods.maskEmail = function () {
    const [user, domain] = this.email.split("@");
    return user[0] + "****@" + domain;
  };
}

userSchema.plugin(maskEmailPlugin);
```

Then in usage:

```js
const user = await User.findById(id);
console.log(user.maskEmail()); // s****@gmail.com
```

---

### ✅ Popular Community Plugins

| Plugin                                                                               | Purpose                                  |
| ------------------------------------------------------------------------------------ | ---------------------------------------- |
| [mongoose-unique-validator](https://www.npmjs.com/package/mongoose-unique-validator) | Better error messages for `unique: true` |
| [mongoose-autopopulate](https://www.npmjs.com/package/mongoose-autopopulate)         | Auto-populate refs                       |
| [mongoose-paginate-v2](https://www.npmjs.com/package/mongoose-paginate-v2)           | Simplified pagination                    |
| [mongoose-soft-delete](https://www.npmjs.com/package/mongoose-delete)                | Add soft-delete (isDeleted flag) support |

---

## 🚀 Real-World Project Use

Let’s say you're building an **E-commerce app**, you’ll need:

- Aggregation for dashboard analytics
- Transactions for order and payment
- Plugins for audit logs or masking
- Virtuals for computed price with tax
- Indexes on `product.slug` or `user.email` for quick lookups

---

**production-ready Mongoose Boilerplate** structure that includes:

- ✅ Advanced Mongoose Features: Transactions, Aggregation, Plugins, Virtuals, Indexes
- 🔒 Modular Architecture: Models, Routes, Controllers, Services
- 🚀 Best Practices: Separation of concerns, validations, reusability, plugins

---

## 📁 Folder Structure

```
mongoose-app/
│
├── config/
│   └── db.js                 # Mongoose connection
│
├── models/
│   ├── user.model.js        # User schema with virtuals, indexes
│   └── order.model.js       # Order schema with transaction support
│
├── plugins/
│   └── maskEmail.js         # Custom plugin
│
├── controllers/
│   └── user.controller.js   # Handles API logic
│
├── services/
│   └── user.service.js      # Handles DB logic and aggregation
│
├── routes/
│   └── user.routes.js       # Express routes
│
├── utils/
│   └── validateObjectId.js  # Middleware for ObjectId check
│
├── .env
├── server.js                # Entry point
└── package.json
```

---

## 🧠 1. `config/db.js` — MongoDB Connection

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("✅ MongoDB Connected");
  } catch (err) {
    console.error("❌ DB Connection Error:", err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

---

## 🧱 2. `models/user.model.js` — Schema with All Features

```js
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");
const maskEmailPlugin = require("../plugins/maskEmail");

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: true,
      minlength: 3,
      trim: true,
    },
    email: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
      match: [/.+@.+\..+/, "Invalid email"],
    },
    password: {
      type: String,
      required: true,
      minlength: 6,
      select: false,
    },
    role: {
      type: String,
      enum: ["user", "admin"],
      default: "user",
      index: true,
    },
    balance: {
      type: Number,
      default: 0,
      min: 0,
    },
  },
  { timestamps: true }
);

// 🔐 Hash password before saving
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();
  this.password = await bcrypt.hash(this.password, 10);
  next();
});

// 👤 Add custom instance method
userSchema.methods.comparePassword = function (candidate) {
  return bcrypt.compare(candidate, this.password);
};

// 🧮 Virtual field: ID + Role
userSchema.virtual("profile").get(function () {
  return `${this.name} (${this.role})`;
});

// 🔌 Custom plugin: mask email
userSchema.plugin(maskEmailPlugin);

module.exports = mongoose.model("User", userSchema);
```

---

## 🧩 3. `plugins/maskEmail.js`

```js
module.exports = function (schema) {
  schema.methods.maskEmail = function () {
    const [u, d] = this.email.split("@");
    return u[0] + "****@" + d;
  };
};
```

---

## 📦 4. `models/order.model.js` — With Transaction Use

```js
const mongoose = require("mongoose");

const orderSchema = new mongoose.Schema({
  buyer: { type: mongoose.Schema.Types.ObjectId, ref: "User", required: true },
  total: { type: Number, required: true },
  status: {
    type: String,
    enum: ["pending", "paid", "shipped"],
    default: "pending",
  },
});

module.exports = mongoose.model("Order", orderSchema);
```

---

## 🧠 5. `services/user.service.js` — Aggregation, Transactions

```js
const User = require("../models/user.model");
const Order = require("../models/order.model");
const mongoose = require("mongoose");

exports.transferMoney = async (senderId, receiverId, amount) => {
  const session = await mongoose.startSession();

  try {
    session.startTransaction();

    const sender = await User.findById(senderId).session(session);
    const receiver = await User.findById(receiverId).session(session);

    if (sender.balance < amount) throw new Error("Insufficient balance");

    sender.balance -= amount;
    receiver.balance += amount;

    await sender.save({ session });
    await receiver.save({ session });

    await session.commitTransaction();
    return { success: true };
  } catch (err) {
    await session.abortTransaction();
    throw err;
  } finally {
    session.endSession();
  }
};

exports.getStatsByRole = async () => {
  return User.aggregate([
    { $group: { _id: "$role", total: { $sum: 1 } } },
    { $sort: { total: -1 } },
  ]);
};
```

---

## 📡 6. `controllers/user.controller.js`

```js
const UserService = require("../services/user.service");

exports.getRoleStats = async (req, res) => {
  try {
    const stats = await UserService.getStatsByRole();
    res.json(stats);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
};

exports.transfer = async (req, res) => {
  const { senderId, receiverId, amount } = req.body;

  try {
    await UserService.transferMoney(senderId, receiverId, amount);
    res.json({ message: "Transfer successful" });
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
};
```

---

## 🛣️ 7. `routes/user.routes.js`

```js
const express = require("express");
const router = express.Router();
const userController = require("../controllers/user.controller");

router.post("/transfer", userController.transfer);
router.get("/stats", userController.getRoleStats);

module.exports = router;
```

---

## 🚀 8. `server.js`

```js
require("dotenv").config();
const express = require("express");
const connectDB = require("./config/db");

const app = express();
app.use(express.json());

// Routes
app.use("/api/users", require("./routes/user.routes"));

connectDB();
const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`🚀 Server running on port ${PORT}`));
```

---

## ✅ 9. `.env`

```
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/mongoose-boilerplate
```

---

## ✅ 10. `package.json` Dependencies

```json
{
  "dependencies": {
    "bcryptjs": "^2.4.3",
    "dotenv": "^16.4.5",
    "express": "^4.19.2",
    "mongoose": "^8.4.0"
  }
}
```

---

## 📌 Features Covered

| Feature           | Implemented                             |
| ----------------- | --------------------------------------- |
| Connect + Env     | ✅ `db.js` + `.env`                     |
| Advanced Schema   | ✅ Custom validators, virtuals, plugins |
| Indexes           | ✅ On role field                        |
| Aggregation       | ✅ User role grouping                   |
| Transactions      | ✅ Money transfer                       |
| Plugins           | ✅ Custom `maskEmail` plugin            |
| Modular Structure | ✅ All logic split cleanly              |

---

# **in-depth yet concise breakdown** of **modern authentication & authorization mechanisms**

## 🔐 1. JWT-Based Authentication (Stateless)

### 🔧 Backend (Express + JWT)

```js
// auth.controller.js
const jwt = require("jsonwebtoken");
const SECRET = "shhh";

exports.login = (req, res) => {
  const { username, password } = req.body;
  // Validate from DB
  const user = { id: 1, username, role: "admin" };
  const token = jwt.sign(user, SECRET, { expiresIn: "15m" });
  const refreshToken = jwt.sign(user, SECRET, { expiresIn: "7d" });

  res.cookie("token", token, { httpOnly: true });
  res.cookie("refresh", refreshToken, { httpOnly: true });
  res.json({ message: "Login successful" });
};

// middleware.js
exports.authenticate = (req, res, next) => {
  const token = req.cookies.token;
  try {
    const user = jwt.verify(token, SECRET);
    req.user = user;
    next();
  } catch {
    res.status(401).json({ message: "Invalid token" });
  }
};
```

### 🧑‍💻 Frontend (React)

```js
// login.jsx
await axios.post(
  "/api/login",
  { username, password },
  { withCredentials: true }
);
```

---

## 🔁 2. JWT + Refresh Token Rotation

### 🔧 Backend

```js
// refresh.controller.js
exports.refresh = (req, res) => {
  const { refresh } = req.cookies;
  try {
    const user = jwt.verify(refresh, SECRET);
    const newToken = jwt.sign(user, SECRET, { expiresIn: "15m" });
    res.cookie("token", newToken, { httpOnly: true });
    res.json({ message: "Token refreshed" });
  } catch {
    res.status(403).json({ message: "Refresh expired" });
  }
};
```

---

## 🧷 3. Session-Based Authentication (Server Memory/Redis)

```js
// session-setup.js
const session = require("express-session");
app.use(session({ secret: "xyz", resave: false, saveUninitialized: true }));

app.post("/login", (req, res) => {
  // Validate user
  req.session.user = { id: 1, name: "shubham" };
  res.send("Logged in");
});

app.get("/dashboard", (req, res) => {
  if (req.session.user) res.send("Welcome");
  else res.status(401).send("Unauthorized");
});
```

---

## 🔐 4. OAuth 2.0 + OpenID Connect (e.g., Google Login)

### 🔧 Frontend

```js
// google-login.jsx
window.open("http://localhost:3000/auth/google", "_self");
```

### 🔧 Backend (Passport.js)

```js
// google.strategy.js
const passport = require("passport");
const GoogleStrategy = require("passport-google-oauth20").Strategy;

passport.use(
  new GoogleStrategy(
    {
      clientID: "GOOGLE_ID",
      clientSecret: "SECRET",
      callbackURL: "/auth/google/callback",
    },
    (accessToken, refreshToken, profile, done) => {
      return done(null, profile);
    }
  )
);

// routes
app.get(
  "/auth/google",
  passport.authenticate("google", { scope: ["profile", "email"] })
);
app.get(
  "/auth/google/callback",
  passport.authenticate("google", { session: false }),
  (req, res) => {
    // Generate your own JWT
    const token = jwt.sign({ id: req.user.id }, SECRET);
    res.cookie("token", token, { httpOnly: true }).redirect("/dashboard");
  }
);
```

---

## 👥 5. Role-Based Access Control (RBAC)

```js
// middleware.js
exports.authorize = (roles = []) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role))
      return res.status(403).send("Forbidden");
    next();
  };
};

// usage
app.get("/admin", authenticate, authorize(["admin"]), (req, res) => {
  res.send("Admin panel");
});
```

---

## 🔐 6. Passwordless Auth (Magic Link)

### High-level flow:

1. User enters email.
2. Backend sends secure link with signed token.
3. User clicks → backend verifies → sets session/token.

```js
// magic.controller.js
const token = jwt.sign({ email }, SECRET, { expiresIn: "10m" });
const link = `https://yourdomain.com/verify?token=${token}`;
// Send link via email

app.get("/verify", (req, res) => {
  try {
    const { email } = jwt.verify(req.query.token, SECRET);
    // create session / token
    res.send("Logged in with magic link");
  } catch {
    res.status(400).send("Invalid/expired link");
  }
});
```

---

## 🛡 7. WebAuthn (Passwordless + Biometric Login)

- Uses hardware-based key pairs (FIDO2 standard)
- Implement with [@simplewebauthn](https://github.com/MasterKale/SimpleWebAuthn) or [WebAuthn.io](https://webauthn.io)

---

## 🧠 8. Attribute-Based Access Control (ABAC)

```js
// middleware
exports.checkAccess = (ruleFn) => {
  return (req, res, next) => {
    const user = req.user;
    if (!ruleFn(user)) return res.status(403).send("Denied");
    next();
  };
};

// Example usage
app.get(
  "/resource",
  authenticate,
  checkAccess((user) => user.department === "finance"),
  (req, res) => {
    res.send("Access granted");
  }
);
```

---

## 🔒 Secure Implementation Best Practices

| Practice                            | Details                                      |
| ----------------------------------- | -------------------------------------------- |
| ✅ `HttpOnly`, `Secure`, `SameSite` | For cookies storing tokens                   |
| ✅ CSRF protection                  | Use tokens or SameSite cookies               |
| ✅ XSS prevention                   | Escape output, use `helmet`, avoid inline JS |
| ✅ Use HTTPS only                   | TLS is mandatory for auth flows              |
| ✅ Token expiration                 | Access: short (15m), Refresh: long (7d)      |
| ✅ Brute-force protection           | Add `express-rate-limit`                     |
| ✅ Store password securely          | Use `bcrypt` with salt                       |

---

## 🔌 Auth Libraries

| Tool                       | Use                                    |
| -------------------------- | -------------------------------------- |
| **Passport.js**            | OAuth strategies                       |
| **jsonwebtoken**           | JWT signing/verification               |
| **NextAuth.js**            | Full Next.js authentication            |
| **Firebase Auth**          | Serverless auth (email, Google, phone) |
| **Clerk/Auth0/Magic.link** | Modern plug-n-play auth as a service   |

---
