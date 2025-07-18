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
    next(err);
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

You're welcome, Shubham! Now let’s go **in-depth into routing in Node.js with Express.js**, including:

- **Basic Routing**
- **Dynamic Routing**
- **Nested & Modular Routing**
- **Route Parameters vs Query Strings**
- **Middleware per Route**
- **HTTP Methods**
- **Controller-based routing**
- **Best Practices**

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

✅ Separation of concerns
✅ Reusability (e.g. auth, logging)
✅ Centralized logic (e.g. error handling)
✅ Cleaner code in route handlers

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

# 📦 What is Mongoose?

> Mongoose is an **Object Data Modeling (ODM)** library for MongoDB and Node.js. It provides:

- Schema-based modeling
- Data validation
- Middleware/hooks
- Query building with chaining
- Relationship support

---

# 🧱 Step-by-Step: Mongoose + Express Integration

---

## ✅ 1. Setup Project

```bash
mkdir express-mongoose-app && cd express-mongoose-app
npm init -y
npm install express mongoose
```

---

## ✅ 2. Create Folder Structure

```
express-mongoose-app/
│
├── models/         ← Mongoose schemas/models
│   └── User.js
│
├── routes/         ← Express routers
│   └── userRoutes.js
│
├── controllers/    ← Logic separated from routes
│   └── userController.js
│
├── app.js          ← Main entry
└── .env            ← DB connection string
```

---

## ✅ 3. Connect to MongoDB using Mongoose

```js
// app.js
const express = require("express");
const mongoose = require("mongoose");
const userRoutes = require("./routes/userRoutes");
require("dotenv").config();

const app = express();

// Middlewares
app.use(express.json());
app.use("/api/users", userRoutes);

// Connect to DB
mongoose
  .connect(process.env.MONGO_URI || "mongodb://localhost:27017/testdb")
  .then(() => {
    console.log("MongoDB connected");
    app.listen(3000, () => console.log("Server running on port 3000"));
  })
  .catch((err) => console.error("MongoDB connection error:", err));
```

---

## ✅ 4. Define Schema and Model

```js
// models/User.js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, "Name is required"],
    minlength: 3,
  },
  email: {
    type: String,
    required: true,
    unique: true,
  },
  age: Number,
  createdAt: {
    type: Date,
    default: Date.now,
  },
});

module.exports = mongoose.model("User", userSchema);
```

---

## ✅ 5. Create Controller Functions

```js
// controllers/userController.js
const User = require("../models/User");

// GET all users
exports.getUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};

// POST create new user
exports.createUser = async (req, res) => {
  try {
    const user = await User.create(req.body);
    res.status(201).json(user);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
};

// PUT update
exports.updateUser = async (req, res) => {
  const { id } = req.params;
  const updated = await User.findByIdAndUpdate(id, req.body, { new: true });
  res.json(updated);
};

// DELETE user
exports.deleteUser = async (req, res) => {
  await User.findByIdAndDelete(req.params.id);
  res.json({ message: "User deleted" });
};
```

---

## ✅ 6. Setup Routes

```js
// routes/userRoutes.js
const express = require("express");
const router = express.Router();
const userController = require("../controllers/userController");

router.get("/", userController.getUsers);
router.post("/", userController.createUser);
router.put("/:id", userController.updateUser);
router.delete("/:id", userController.deleteUser);

module.exports = router;
```

---

## ✅ 7. Example `.env` File

```
MONGO_URI=mongodb://localhost:27017/userapp
```

Use with:

```bash
npm install dotenv
```

---

# 🔁 Mongoose Model Methods

| Method                            | Description  |
| --------------------------------- | ------------ |
| `User.find()`                     | Get all      |
| `User.findById(id)`               | Get by ID    |
| `User.create(obj)`                | Insert       |
| `User.findByIdAndUpdate(id, obj)` | Update       |
| `User.findByIdAndDelete(id)`      | Delete       |
| `User.findOne({ email })`         | Query single |

---

# 🧪 Bonus: Schema Validations

```js
email: {
  type: String,
  required: true,
  match: /.+\@.+\..+/,
  unique: true
}
```

---

# 🔗 Real World Use Case: Auth

- User Registration → `User.create()`
- Login → `User.findOne({ email })`
- Reset Password → `User.updateOne()`

---

# ✅ Summary

| Task        | Code                                        |
| ----------- | ------------------------------------------- |
| Setup       | `mongoose.connect(...)`                     |
| Schema      | `new mongoose.Schema({})`                   |
| Model       | `mongoose.model('User', schema)`            |
| CRUD        | `create`, `find`, `findByIdAndUpdate`, etc. |
| Integration | Use in controller functions and route       |

---

## 🔷 What is a Mongoose Schema?

> A **Schema** in Mongoose defines the **structure**, **default values**, **validators**, and **types** of the data in a MongoDB collection.

It's like a blueprint for documents in a collection.

---

## 🔧 Basic Setup Example

```js
const mongoose = require("mongoose");

// Step 1: Define the schema
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  age: Number,
});

// Step 2: Create the model
const User = mongoose.model("User", userSchema);
```

---

## 🧠 Field Types in Schema

Mongoose supports most JavaScript data types:

| Type       | Description                                        |
| ---------- | -------------------------------------------------- |
| `String`   | Text                                               |
| `Number`   | Numeric                                            |
| `Boolean`  | true/false                                         |
| `Date`     | Date value                                         |
| `Buffer`   | Binary data (file/image)                           |
| `ObjectId` | MongoDB ObjectID (for referencing)                 |
| `Array`    | List of values                                     |
| `Mixed`    | Any type of data (not recommended for strict apps) |
| `Map`      | Key-value pairs                                    |

---

## ✍️ Full Schema with Options

```js
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true,
    trim: true,
    minlength: 2,
  },
  email: {
    type: String,
    required: true,
    unique: true,
    lowercase: true,
  },
  password: {
    type: String,
    required: true,
    select: false, // hide from queries by default
  },
  age: {
    type: Number,
    default: 18,
  },
  isAdmin: {
    type: Boolean,
    default: false,
  },
  createdAt: {
    type: Date,
    default: Date.now,
  },
});
```

---

## 🧪 Real-world Example: Product Schema

```js
const productSchema = new mongoose.Schema({
  title: {
    type: String,
    required: [true, "Product title is required"],
  },
  price: {
    type: Number,
    required: true,
    min: [0, "Price must be positive"],
  },
  tags: [String], // array of strings
  inStock: {
    type: Boolean,
    default: true,
  },
});
```

---

## 🧩 Schema Methods

### 🔹 Instance Methods

```js
userSchema.methods.isAdult = function () {
  return this.age >= 18;
};
```

Usage:

```js
const user = await User.findById(id);
if (user.isAdult()) {
  // do something
}
```

---

### 🔹 Static Methods

```js
userSchema.statics.findByEmail = function (email) {
  return this.findOne({ email });
};
```

Usage:

```js
const user = await User.findByEmail("test@example.com");
```

---

### 🔹 Virtuals (computed fields)

```js
userSchema.virtual("fullName").get(function () {
  return this.firstName + " " + this.lastName;
});
```

> Virtuals don’t get stored in DB but are accessible like normal fields.

---

## 🧹 Middleware (Hooks)

```js
userSchema.pre("save", function (next) {
  this.updatedAt = Date.now();
  next();
});
```

Other middleware types:
`pre/post` for `save`, `remove`, `find`, `updateOne`, etc.

---

## 🔄 Nested Schema

```js
const addressSchema = new mongoose.Schema({
  city: String,
  zip: String,
});

const userSchema = new mongoose.Schema({
  name: String,
  address: addressSchema,
});
```

---

## 🔗 Schema for Referencing (ObjectId)

```js
const postSchema = new mongoose.Schema({
  title: String,
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
  },
});
```

Then, when querying:

```js
Post.find().populate("author");
```

---

## ✅ Summary

| Feature                 | Use                              |
| ----------------------- | -------------------------------- |
| `type`                  | Defines the data type            |
| `required`, `unique`    | Validators                       |
| `default`, `min`, `max` | Constraints                      |
| `methods`, `statics`    | Add custom logic                 |
| `virtuals`              | Computed fields                  |
| `hooks`                 | Lifecycle middleware             |
| `ObjectId` + `ref`      | Relationship between collections |

---

## 🔹 1. **What are Headers?**

**HTTP headers** are key-value pairs sent in requests and responses to pass **metadata** like:

- Content-Type
- Authorization tokens
- Cookies
- User-Agent
- CORS permissions
- Custom app-specific headers

---

## 🔹 2. **Reading Request Headers in Express**

You can read headers from the request using `req.headers`, `req.get()`, or `req.header()`:

```js
app.get("/test", (req, res) => {
  console.log(req.headers); // All headers
  const authHeader = req.get("Authorization"); // OR req.headers['authorization']
  res.send("Check console");
});
```

🔸 Header keys are **case-insensitive**.

---

## 🔹 3. **Setting Response Headers in Express**

Use `res.set()` or `res.header()` or `res.setHeader()`:

```js
app.get("/download", (req, res) => {
  res.set("Content-Type", "application/json");
  res.set("X-Powered-By", "ShubhamExpressAPI");
  res.send({ msg: "Headers set" });
});
```

---

## 🔹 4. **Common Headers You May Use**

| Type     | Header                                                | Use Case                              |
| -------- | ----------------------------------------------------- | ------------------------------------- |
| CORS     | `Access-Control-Allow-Origin`                         | Allow frontend to access APIs         |
| Auth     | `Authorization: Bearer xyz`                           | JWT or Basic Auth                     |
| Content  | `Content-Type: application/json`                      | Tells server what format body is      |
| Cookies  | `Cookie` / `Set-Cookie`                               | Used for session/token authentication |
| Security | `Strict-Transport-Security`, `X-Content-Type-Options` | Harden app from attacks               |
| Caching  | `Cache-Control`, `ETag`                               | Manage browser caching                |
| Custom   | `X-App-Version`, etc.                                 | For custom logic or version control   |

---

## 🔹 5. **Send Custom Headers in Response**

```js
res.set({
  "X-API-Version": "1.0",
  "X-Auth-Required": "true",
});
```

---

## 🔹 6. **Middleware for Header-Based Logic**

You can create middleware to check headers:

```js
const checkAuthHeader = (req, res, next) => {
  const token = req.get("Authorization");
  if (!token) return res.status(401).json({ msg: "No token" });
  // do more validation...
  next();
};

app.use("/protected", checkAuthHeader);
```

---

## 🔹 7. **Security Headers Using Helmet (Best Practice)**

```bash
npm install helmet
```

```js
const helmet = require("helmet");
app.use(helmet()); // Automatically sets many security headers
```

✅ This adds:

- `X-Frame-Options`
- `Strict-Transport-Security`
- `X-Content-Type-Options`
- `Content-Security-Policy` (with config)

---

## 🔹 8. **Setting Headers for File Download**

```js
app.get("/file", (req, res) => {
  res.setHeader("Content-Disposition", "attachment; filename=info.txt");
  res.send("Your download will start");
});
```

---

## 🔹 9. **Sending Headers from React**

```js
axios.get("/protected", {
  headers: {
    Authorization: `Bearer ${token}`,
    "X-Client": "ReactApp",
  },
});
```

---

## ✅ Summary

| Operation           | Use                              |
| ------------------- | -------------------------------- |
| Read request header | `req.get("HeaderName")`          |
| Set response header | `res.set("HeaderName", "value")` |
| Security headers    | Use Helmet                       |
| Auth check          | Middleware on headers            |
| Cross-origin        | Set `Access-Control-*`           |

---

## 🍪 What Are Cookies?

**Cookies** are small pieces of data stored on the client (browser) and sent to the server with every HTTP request. They're commonly used to:

- Store session IDs
- Track user preferences
- Handle login states
- Store tokens or user info (lightweight)

---

## ⚙️ How to Use Cookies in Express

### Step 1: Install `cookie-parser`

```bash
npm install cookie-parser
```

### Step 2: Use it as middleware

```js
const express = require("express");
const cookieParser = require("cookie-parser");

const app = express();
app.use(cookieParser());
```

---

## ✅ Set a Cookie

```js
app.get("/set-cookie", (req, res) => {
  res.cookie("username", "Shubham", {
    maxAge: 1000 * 60 * 60, // 1 hour
    httpOnly: true, // Can’t be accessed via JS (prevents XSS)
    secure: true, // Only sent over HTTPS
    sameSite: "strict", // Prevent CSRF
  });
  res.send("Cookie has been set");
});
```

---

## 📥 Read a Cookie

```js
app.get("/get-cookie", (req, res) => {
  const username = req.cookies.username;
  res.send(`Hello, ${username}`);
});
```

---

## ❌ Clear a Cookie

```js
app.get("/clear-cookie", (req, res) => {
  res.clearCookie("username");
  res.send("Cookie cleared");
});
```

---

## 🛡️ Why Use Cookies for Security?

### 🔐 1. **Session Management**

- Store a **session ID** in cookie
- On the server, use that ID to retrieve session data

> ✅ Use libraries like `express-session` for secure session management

---

### 🔐 2. **Prevent XSS (Cross Site Scripting)**

```js
httpOnly: true;
```

> Ensures that malicious JS can't access cookies.

---

### 🔐 3. **Prevent CSRF (Cross Site Request Forgery)**

```js
sameSite: "strict"; // or 'lax'
```

> Tells browser not to send cookies on cross-origin requests.

---

### 🔐 4. **Secure Transmission**

```js
secure: true;
```

> Only send cookies via **HTTPS**.

---

## ⚠️ Never Store Sensitive Info in Cookies

Do **not** store:

- Passwords
- Tokens (unless encrypted)
- Credit card numbers

> Use cookies to store lightweight identifiers (like session IDs or signed JWTs).

---

## 📦 Real-World Example: Session Authentication

```js
// Using cookie-parser + JWT
const jwt = require("jsonwebtoken");

app.post("/login", (req, res) => {
  const user = { id: 1, username: "Shubham" };
  const token = jwt.sign(user, "secret", { expiresIn: "1h" });

  res.cookie("token", token, {
    httpOnly: true,
    secure: true,
    sameSite: "strict",
    maxAge: 3600000,
  });

  res.send("Logged in");
});
```

---

## 🧪 Debug Cookies

In the browser:

- DevTools → Application → Cookies

---

## 🔍 Summary Table

| Feature             | Code Option           | Purpose                 |
| ------------------- | --------------------- | ----------------------- |
| `httpOnly`          | `true`                | Prevent JS access (XSS) |
| `secure`            | `true`                | Only send via HTTPS     |
| `sameSite`          | `'strict'` or `'lax'` | Prevent CSRF            |
| `maxAge`            | `in ms`               | Cookie expiration       |
| `res.cookie()`      | Set cookie            |                         |
| `res.clearCookie()` | Remove cookie         |                         |

---

## 🔑 What is `express-session`?

`express-session` is a middleware that stores session data on the server. Each user gets a unique session ID stored in a cookie (`connect.sid`). On future requests, the server uses that ID to retrieve the session.

---

## 📦 1. Installation

```bash
npm install express-session
```

---

## ⚙️ 2. Basic Setup

```js
const express = require("express");
const session = require("express-session");

const app = express();

// Middleware
app.use(
  session({
    secret: "your_super_secret_key", // Used to sign the session ID cookie
    resave: false, // Don’t save session if unmodified
    saveUninitialized: false, // Don’t create session until something stored
    cookie: {
      httpOnly: true,
      secure: false, // Set true in production (HTTPS)
      maxAge: 1000 * 60 * 60, // 1 hour
    },
  })
);
```

---

## ✅ 3. How It Works

### Set data in session

```js
app.get("/login", (req, res) => {
  req.session.user = {
    id: 1,
    username: "Shubham",
  };
  res.send("Session set!");
});
```

### Access session data

```js
app.get("/profile", (req, res) => {
  if (req.session.user) {
    res.send(`Welcome ${req.session.user.username}`);
  } else {
    res.status(401).send("Unauthorized");
  }
});
```

### Destroy session (logout)

```js
app.get("/logout", (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      return res.send("Logout failed");
    }
    res.clearCookie("connect.sid"); // remove session ID from browser
    res.send("Logged out");
  });
});
```

---

## 🔐 4. Session Security Best Practices

| Option               | Why                                    |
| -------------------- | -------------------------------------- |
| `secret`             | Signs the cookie, preventing tampering |
| `httpOnly: true`     | Prevent JS access (XSS)                |
| `secure: true`       | Send cookie only over HTTPS            |
| `sameSite: 'strict'` | Mitigate CSRF attacks                  |
| `maxAge`             | Auto-logout after inactivity           |

🔐 **Production Config:**

```js
cookie: {
  httpOnly: true,
  secure: true,         // Set to true on HTTPS
  sameSite: 'strict',
  maxAge: 1000 * 60 * 60
}
```

---

## 🧠 5. MemoryStore Warning

> `express-session` uses in-memory storage by default — not recommended for production due to memory leaks and no persistence.

✅ **Use a store like:**

- `connect-mongo` (MongoDB)
- `connect-redis` (Redis)
- `connect-sqlite3`, `connect-pg-simple`, etc.

---

## 🧰 6. MongoDB Store Example (using `connect-mongo`)

```bash
npm install connect-mongo
```

```js
const MongoStore = require("connect-mongo");

app.use(
  session({
    secret: "your_super_secret_key",
    resave: false,
    saveUninitialized: false,
    store: MongoStore.create({
      mongoUrl: "mongodb://localhost:27017/your-db",
      ttl: 60 * 60, // 1 hour
    }),
    cookie: {
      httpOnly: true,
      secure: true,
      maxAge: 1000 * 60 * 60,
    },
  })
);
```

---

## 🧪 7. Session Flow Summary

1. First request hits `/login`, session created and stored.
2. Cookie `connect.sid` is sent to browser.
3. Subsequent requests send that cookie.
4. Express uses the ID to fetch session data.
5. Session can be updated/destroyed anytime.

---

## 📁 Session Storage Example (in-memory)

```js
req.session = {
  id: "abc123",
  user: {
    id: 1,
    name: "Shubham",
  },
  isAuthenticated: true,
};
```

---

## 🔍 DevTools Debugging

- In Chrome DevTools → Application → Cookies → `connect.sid`

---

## ✅ Real World Uses

- **Login systems**
- **Cart systems** (store items in `req.session.cart`)
- **Flash messages**
- **Multi-step forms**

---

## 🧠 1. What is Session-Based Authentication?

- When a user logs in, the server stores their login info in a **session (in memory or DB)**.
- The server sends a **session ID (cookie)** to the browser.
- On each request, the cookie is sent back → used to identify the user.
- Unlike JWT (stateless), sessions are **stateful** and stored **on the server**.

---

## 🧱 2. Project Setup

```bash
npm init -y
npm install express mongoose express-session connect-mongo bcryptjs dotenv
```

### File structure:

```
project/
├── models/User.js
├── routes/auth.js
├── app.js
└── .env
```

---

## 🗄️ 3. Mongoose User Model (`models/User.js`)

```js
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const userSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  password: { type: String, required: true },
});

// Hash password before saving
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();
  this.password = await bcrypt.hash(this.password, 12);
  next();
});

module.exports = mongoose.model("User", userSchema);
```

---

## 🌐 4. Express App (`app.js`)

```js
const express = require("express");
const mongoose = require("mongoose");
const session = require("express-session");
const MongoStore = require("connect-mongo");
const authRoutes = require("./routes/auth");
require("dotenv").config();

const app = express();

app.use(express.urlencoded({ extended: true }));
app.use(express.json());

// Session config
app.use(
  session({
    secret: process.env.SESSION_SECRET,
    resave: false,
    saveUninitialized: false,
    store: MongoStore.create({ mongoUrl: process.env.MONGO_URI }),
    cookie: {
      httpOnly: true,
      secure: false, // Set true in production (HTTPS)
      maxAge: 1000 * 60 * 60 * 1, // 1 hour
    },
  })
);

// Routes
app.use("/auth", authRoutes);

mongoose
  .connect(process.env.MONGO_URI)
  .then(() =>
    app.listen(3000, () => console.log("Running on http://localhost:3000"))
  )
  .catch(console.error);
```

---

## 🔐 5. Auth Routes (`routes/auth.js`)

```js
const express = require("express");
const bcrypt = require("bcryptjs");
const User = require("../models/User");
const router = express.Router();

// Register
router.post("/register", async (req, res) => {
  try {
    const { username, password } = req.body;
    const user = new User({ username, password });
    await user.save();
    res.status(201).send("Registered successfully");
  } catch (err) {
    res.status(400).send("Error: " + err.message);
  }
});

// Login
router.post("/login", async (req, res) => {
  const { username, password } = req.body;
  const user = await User.findOne({ username });
  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).send("Invalid credentials");
  }

  req.session.userId = user._id; // Save session
  res.send("Login successful");
});

// Logout
router.get("/logout", (req, res) => {
  req.session.destroy((err) => {
    if (err) return res.status(500).send("Logout error");
    res.clearCookie("connect.sid");
    res.send("Logged out");
  });
});

// Protected route
router.get("/profile", async (req, res) => {
  if (!req.session.userId) return res.status(401).send("Login first");
  const user = await User.findById(req.session.userId).select("-password");
  res.send(user);
});

module.exports = router;
```

---

## 🔐 6. Middleware to Protect Routes

```js
const isAuthenticated = (req, res, next) => {
  if (req.session.userId) return next();
  return res.status(401).send("Access denied");
};
```

---

## 🛡️ 7. Security Best Practices

| Best Practice                    | Why Important                             |
| -------------------------------- | ----------------------------------------- |
| Use HTTPS in production          | To secure cookies & session data          |
| Set `httpOnly` and `secure`      | Prevents client-side JS access to cookies |
| Store session in DB (not memory) | Safer and scalable                        |
| Rotate session secret frequently | Avoid hijacking                           |
| Use `express-rate-limit`         | To avoid brute-force login attacks        |

---

## ✅ Sample `.env`

```env
MONGO_URI=mongodb://localhost:27017/sessions_demo
SESSION_SECRET=mySuperSecretKey
```

---

## 🧪 Test with Postman or Browser

- **POST /auth/register** `{ "username": "shubham", "password": "123456" }`
- **POST /auth/login**
- **GET /auth/profile** — protected
- **GET /auth/logout**

---

## 🔐 Why JWT for Authentication?

JWT is a **stateless**, secure way to authenticate users without server-side sessions.

- ✅ Self-contained token with user info (like ID)
- ✅ Sent with every request via headers
- ✅ Stateless: No need to store session data on the server

---

## 🏗️ Project Structure

```
project/
├── server.js
├── routes/
│   └── auth.js
├── controllers/
│   └── authController.js
├── middlewares/
│   └── authMiddleware.js
├── models/
│   └── User.js
├── config/
│   └── db.js
└── .env
```

---

## 🧱 Step-by-Step Setup

---

### ✅ Step 1: Install Required Packages

```bash
npm install express mongoose dotenv bcryptjs jsonwebtoken cookie-parser
```

---

### ✅ Step 2: Environment Setup (`.env`)

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
JWT_EXPIRES_IN=1d
```

---

### ✅ Step 3: Connect to MongoDB (`config/db.js`)

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB Connected");
  } catch (error) {
    console.error(error.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

---

### ✅ Step 4: Create User Schema (`models/User.js`)

```js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  username: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
});

module.exports = mongoose.model("User", userSchema);
```

---

### ✅ Step 5: Auth Controller (`controllers/authController.js`)

```js
const User = require("../models/User");
const bcrypt = require("bcryptjs");
const jwt = require("jsonwebtoken");

// Register
exports.register = async (req, res) => {
  const { username, email, password } = req.body;
  try {
    const hashedPassword = await bcrypt.hash(password, 10);
    const newUser = await User.create({
      username,
      email,
      password: hashedPassword,
    });
    res.status(201).json({ message: "User registered", userId: newUser._id });
  } catch (error) {
    res.status(500).json({ error: "User registration failed" });
  }
};

// Login
exports.login = async (req, res) => {
  const { email, password } = req.body;
  try {
    const user = await User.findOne({ email });
    if (!user) return res.status(401).json({ error: "User not found" });

    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(401).json({ error: "Invalid password" });

    const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET, {
      expiresIn: process.env.JWT_EXPIRES_IN,
    });

    // Optionally set as HTTP-only cookie
    res
      .cookie("token", token, {
        httpOnly: true,
        secure: process.env.NODE_ENV === "production",
        sameSite: "strict",
        maxAge: 24 * 60 * 60 * 1000,
      })
      .json({ message: "Login successful", token });
  } catch (error) {
    res.status(500).json({ error: "Login failed" });
  }
};
```

---

### ✅ Step 6: Middleware to Protect Routes (`middlewares/authMiddleware.js`)

```js
const jwt = require("jsonwebtoken");

exports.verifyToken = (req, res, next) => {
  const token = req.cookies.token || req.headers["authorization"];
  if (!token) return res.status(401).json({ error: "Access denied" });

  try {
    const decoded = jwt.verify(
      token.replace("Bearer ", ""),
      process.env.JWT_SECRET
    );
    req.user = decoded;
    next();
  } catch (err) {
    res.status(401).json({ error: "Invalid token" });
  }
};
```

---

### ✅ Step 7: Auth Routes (`routes/auth.js`)

```js
const express = require("express");
const router = express.Router();
const { register, login } = require("../controllers/authController");
const { verifyToken } = require("../middlewares/authMiddleware");

router.post("/register", register);
router.post("/login", login);

// Protected Route
router.get("/me", verifyToken, (req, res) => {
  res.json({ message: "Protected route accessed", user: req.user });
});

module.exports = router;
```

---

### ✅ Step 8: Main Server File (`server.js`)

```js
require("dotenv").config();
const express = require("express");
const app = express();
const cookieParser = require("cookie-parser");
const connectDB = require("./config/db");

app.use(express.json());
app.use(cookieParser());

connectDB();

app.use("/api/auth", require("./routes/auth"));

app.listen(process.env.PORT, () =>
  console.log(`Server running on port ${process.env.PORT}`)
);
```

---

## 🔐 Security Best Practices

| Practice                                                 | Why                                   |
| -------------------------------------------------------- | ------------------------------------- |
| ✅ `httpOnly` cookies                                    | Prevents JavaScript access (XSS-safe) |
| ✅ Use `secure: true` in production                      | Cookie only over HTTPS                |
| ✅ Short expiry (`1d`) + refresh                         | Limits damage if token is stolen      |
| ✅ Store token in `Authorization` header for mobile APIs | More flexible for clients             |

---

## ✅ Final API Example

- `POST /api/auth/register` — Register new user
- `POST /api/auth/login` — Login and receive JWT
- `GET /api/auth/me` — Protected route (need JWT)

---

## ✅ 1. **Pass JWT in Headers (React → Express)**

### 🔸 Using `fetch`:

```js
const token = localStorage.getItem("accessToken");

fetch("http://localhost:5000/api/user", {
  method: "GET",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${token}`,
  },
})
  .then((res) => res.json())
  .then((data) => console.log(data));
```

---

### 🔸 Using `axios`:

```js
import axios from "axios";

const token = localStorage.getItem("accessToken");

axios
  .get("http://localhost:5000/api/user", {
    headers: {
      Authorization: `Bearer ${token}`,
    },
  })
  .then((res) => console.log(res.data))
  .catch((err) => console.error(err));
```

---

## ✅ 2. **Refresh Token Flow**

You typically store:

- `accessToken` → Short-lived (15 min)
- `refreshToken` → Long-lived (7 days)

### ⛓️ Step-by-step:

#### Backend (Express):

- On login, send both tokens.
- Save `refreshToken` in HTTP-only cookie.
- Protect routes with `accessToken`.
- Add `/refresh` route to issue new accessToken when expired.

#### Frontend:

```js
// Example: auto-refresh on 401
axios.interceptors.response.use(
  (res) => res,
  async (err) => {
    if (err.response.status === 401 && !err.config._retry) {
      err.config._retry = true;
      const res = await axios.post(
        "/auth/refresh",
        {},
        { withCredentials: true }
      );

      localStorage.setItem("accessToken", res.data.accessToken);
      err.config.headers["Authorization"] = `Bearer ${res.data.accessToken}`;
      return axios(err.config);
    }
    return Promise.reject(err);
  }
);
```

---

## ✅ 3. **Logout**

### Frontend:

```js
const handleLogout = async () => {
  await axios.post("/auth/logout", {}, { withCredentials: true });
  localStorage.removeItem("accessToken");
  window.location.href = "/login";
};
```

### Backend:

```js
app.post("/auth/logout", (req, res) => {
  res.clearCookie("refreshToken", { httpOnly: true });
  res.json({ message: "Logged out" });
});
```

---

## ✅ 4. **Role-Based Authentication**

### Backend:

#### Middleware:

```js
const authorizeRole = (...roles) => {
  return (req, res, next) => {
    const user = req.user; // from jwt verification
    if (!roles.includes(user.role)) {
      return res.status(403).json({ message: "Forbidden" });
    }
    next();
  };
};
```

#### Usage:

```js
app.get("/admin", verifyToken, authorizeRole("admin"), (req, res) => {
  res.json({ message: "Welcome Admin" });
});
```

---

### Frontend Example:

```js
const user = JSON.parse(localStorage.getItem("user"));
if (user?.role === "admin") {
  // Show admin routes
}
```

---

## 🔐 Summary

| Feature       | Method                                        |
| ------------- | --------------------------------------------- |
| Pass token    | Axios/Fetch → `Authorization: Bearer <token>` |
| Refresh token | HTTP-only cookie + `/auth/refresh`            |
| Logout        | `clearCookie()` + clear localStorage          |
| Role-based    | Middleware + protected frontend routes        |

---

## ⚙️ Overall Flow

1. On **login**, server sets:
   - `accessToken` in memory or response body (optional)
   - `refreshToken` in a **`HttpOnly`**, **`Secure`** cookie
2. React does **not manually pass the token** — browser does it automatically.
3. On **protected routes**, token is verified server-side.
4. If **access token expires**, send request to `/refresh` route → get new token.
5. On **logout**, clear the cookie and token.

---

## 🛡️ 1. Login → Set `HttpOnly` Refresh Token Cookie

### 🔹 Backend (`Express.js`):

```js
// Login Route
app.post("/api/login", async (req, res) => {
  const { email, password } = req.body;

  const user = await User.findOne({ email });
  const valid = user && (await bcrypt.compare(password, user.password));
  if (!valid) return res.status(401).json({ msg: "Invalid credentials" });

  const accessToken = jwt.sign(
    { id: user._id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: "15m" }
  );
  const refreshToken = jwt.sign({ id: user._id }, process.env.REFRESH_SECRET, {
    expiresIn: "7d",
  });

  res
    .cookie("refreshToken", refreshToken, {
      httpOnly: true,
      secure: true, // HTTPS only
      sameSite: "Strict",
      path: "/api/refresh",
      maxAge: 7 * 24 * 60 * 60 * 1000,
    })
    .json({ accessToken, user: { id: user._id, role: user.role } });
});
```

---

## 🌐 2. React Login Request (No Token Handling Needed)

```js
// Login API call from React using Axios or Fetch
await axios.post(
  "https://your-backend.com/api/login",
  { email, password },
  { withCredentials: true } // 👈 important for cookies
);
```

---

## 🔐 3. Protect Routes in Express with `accessToken`

```js
const verifyToken = (req, res, next) => {
  const authHeader = req.headers["authorization"];
  const token = authHeader && authHeader.split(" ")[1];
  if (!token) return res.sendStatus(401);

  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
};
```

---

## 🔁 4. Auto Refresh Access Token Using `/refresh`

### 🔹 Backend:

```js
app.post("/api/refresh", (req, res) => {
  const token = req.cookies.refreshToken;
  if (!token) return res.sendStatus(401);

  jwt.verify(token, process.env.REFRESH_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);

    const newAccessToken = jwt.sign({ id: user.id }, process.env.JWT_SECRET, {
      expiresIn: "15m",
    });
    res.json({ accessToken: newAccessToken });
  });
});
```

---

### 🔹 React Axios Interceptor to Auto-Refresh:

```js
import axios from "axios";

const api = axios.create({
  baseURL: "https://your-backend.com/api",
  withCredentials: true,
});

api.interceptors.response.use(
  (res) => res,
  async (err) => {
    const original = err.config;
    if (err.response?.status === 401 && !original._retry) {
      original._retry = true;
      const res = await api.post("/refresh"); // cookie is auto sent
      localStorage.setItem("accessToken", res.data.accessToken);
      original.headers["Authorization"] = `Bearer ${res.data.accessToken}`;
      return api(original);
    }
    return Promise.reject(err);
  }
);
```

---

## 🔓 5. Logout (Clear Cookie)

### 🔹 Backend:

```js
app.post("/api/logout", (req, res) => {
  res.clearCookie("refreshToken", { path: "/api/refresh" });
  res.status(200).json({ message: "Logged out" });
});
```

### 🔹 React:

```js
await axios.post(
  "https://your-backend.com/api/logout",
  {},
  { withCredentials: true }
);
localStorage.removeItem("accessToken");
navigate("/login");
```

---

## 👮‍♂️ 6. Role-Based Auth (Backend)

### Middleware:

```js
const authorize = (roles = []) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ message: "Forbidden" });
    }
    next();
  };
};
```

### Usage:

```js
app.get("/admin", verifyToken, authorize(["admin"]), (req, res) => {
  res.json({ msg: "Welcome Admin!" });
});
```

---

## 🔐 Security Best Practices

| Feature      | Recommended                                       |
| ------------ | ------------------------------------------------- |
| Cookies      | `HttpOnly`, `Secure`, `SameSite=Strict`           |
| Token Expiry | Access: 15min, Refresh: 7d                        |
| CORS         | Set `credentials: true` on both client and server |
| HTTPS        | Required for `Secure` cookies                     |

---

## 🧪 Test From React (Fetch or Axios)

```js
const accessToken = localStorage.getItem("accessToken");

axios.get("/api/protected", {
  headers: {
    Authorization: `Bearer ${accessToken}`,
  },
  withCredentials: true,
});
```

---

## ✅ Summary Flow:

1. Login → Set `refreshToken` cookie, return `accessToken`
2. Store `accessToken` in localStorage or memory
3. Send it in headers for protected requests
4. If 401, refresh from cookie → get new `accessToken`
5. Logout → clear cookie + localStorage
6. Use `req.user.role` for role-based auth

---

## ✅ What is `withCredentials`?

In **Axios** or **Fetch**, `withCredentials: true` tells the browser to **include credentials** (like cookies, authorization headers, or TLS client certificates) **with cross-origin requests**.

By default, **cross-origin requests don’t send cookies or HTTP auth headers**.

---

## ✅ Why Do We Need `withCredentials`?

If your backend sets a cookie (e.g. `Set-Cookie: token=xyz; HttpOnly; Secure; SameSite=None`) and the frontend doesn't use `withCredentials`, the cookie:

- ❌ Won’t be **saved in the browser**.
- ❌ Won’t be **sent with future requests** (e.g. refresh, protected routes, logout).

That breaks session-based or cookie-based JWT auth.

---

## ✅ When to Use It

| Operation               | When to Use `withCredentials: true` |
| ----------------------- | ----------------------------------- |
| Login (Set Cookie)      | ✅ Required                         |
| Logout (Clear Cookie)   | ✅ Required                         |
| Refresh Token           | ✅ Required                         |
| Access Protected Routes | ✅ Required if server reads cookie  |
| Role-based Auth         | ✅ Required if cookie holds token   |

---

## ✅ Example in Axios

```js
// Login (POST)
axios.post(
  "https://api.example.com/login",
  {
    email: "user@example.com",
    password: "secret",
  },
  {
    withCredentials: true,
  }
);

// Logout (GET or POST)
axios.post(
  "https://api.example.com/logout",
  {},
  {
    withCredentials: true,
  }
);

// Protected route
axios.get("https://api.example.com/user", {
  withCredentials: true,
});
```

---

## ✅ Example in Fetch

```js
// Login
fetch("https://api.example.com/login", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ email, password }),
});

// Logout
fetch("https://api.example.com/logout", {
  method: "POST",
  credentials: "include",
});

// Protected route
fetch("https://api.example.com/user", {
  method: "GET",
  credentials: "include",
});
```

---

## 🔐 Related Backend Settings

- Server must send:

  ```http
  Access-Control-Allow-Credentials: true
  Access-Control-Allow-Origin: https://your-frontend.com
  ```

- Cookie must have:

  ```http
  Set-Cookie: token=abc123; HttpOnly; Secure; SameSite=None;
  ```

---

## 🧠 Why It Matters for Security

- Prevents **XSS**: You store JWT in **HTTP-only cookie**, which JS can’t access.
- Prevents **CSRF** (with additional safeguards).
- Ensures session is maintained across origin.

---
