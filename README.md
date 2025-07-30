## 🧠 What is Node.js

- an `open-source`, `cross-platform`, `runtime environment` that lets you `run JavaScript on the server-side` using the `V8 engine (from Chrome)`.

### ✅ Key Features:

- `Non-blocking I/O model (asynchronous)`
- `Single-threaded event loop`
- `Built-in libraries for HTTP, file system, streams, etc.`
- `NPM (Node Package Manager)` for dependency management
- `Highly scalable` for I/O-bound applications

### ✅ Why use Node.js

| Feature           | Benefit                                                 |
| ----------------- | ------------------------------------------------------- |
| 🧵 Event-driven   | Handles thousands of concurrent connections efficiently |
| ⚡ Fast execution | Powered by V8 engine                                    |
| 🌐 Full-stack JS  | Use JavaScript both on frontend & backend               |
| 🧩 Rich ecosystem | Over 2M packages on NPM                                 |
| 🔁 Real-time      | Great for apps like chat, live dashboards               |

---

## 🔧 What is Express.js

### ✅ Definition:

- a `minimal`, `flexible web application framework` for Node.js that simplifies building web applications and RESTful APIs.
- There are several popular alternatives to ExpressJS which includes:
  - Koa.js
  - Hapi.js
  - Sails.js
  - Fastify

### ✅ Why use Express

| **Feature**          | **Highlights**                                                                 |
| -------------------- | ------------------------------------------------------------------------------ |
| **Routing**          | Simple **URL mapping** for `GET`, `POST`, `PUT`, `DELETE`.                     |
| **Middleware**       | Functions to handle **auth**, **logging**, **body parsing**, etc.              |
| **HTTP Methods**     | Easy handling with **`app.get()`**, **`app.post()`**, etc.                     |
| **Static Files**     | Serve **images, CSS, JS** via `express.static`.                                |
| **Security**         | Use **Helmet**, **CORS**, **rate-limiting** for protection.                    |
| **Template Engines** | Support for **EJS**, **Pug**, **Handlebars** for dynamic views.                |
| **Error Handling**   | Centralized **error middleware** for debugging.                                |
| **Helpers**          | Quick responses with **`res.send()`**, **`res.json()`**, **`res.redirect()`**. |
| **Modularity**       | **Routers** & **modular structure** for scalable apps.                         |
| **REST API Ready**   | Built-in support for **JSON APIs** & RESTful services.                         |

---

### Node.js vs Express.js

| **Aspect**            | **Node.js**                                  | **Express.js**                          |
| --------------------- | -------------------------------------------- | --------------------------------------- |
| **Type**              | Runtime environment for JavaScript           | Framework built on top of Node.js       |
| **Purpose**           | Run JavaScript outside the browser           | Build web servers & APIs quickly        |
| **Level**             | Low-level (you manage server logic manually) | High-level (provides tools & structure) |
| **Built-in features** | Very few (you need to write more code)       | Many (routing, middleware, templates)   |
| **Learning curve**    | Slightly harder (manual setup)               | Easier (predefined methods & structure) |
| **Example**           | `http.createServer()`                        | `app.get('/', (req,res)=>{})`           |

---

## Basic Structure of an Express.js Application

| **Part**                                 | **Purpose**                                                                                  |
| ---------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Entry Point** (`server.js` / `app.js`) | Main **startup file** – sets up server, connects DB, loads middleware, and registers routes. |
| **Routes** (`/routes`)                   | Defines **URL endpoints** (e.g., `/users`, `/products`) and maps them to controllers.        |
| **Controllers** (`/controllers`)         | Contains **business logic** for handling requests & responses.                               |
| **Models** (`/models`)                   | Defines **database schemas** (e.g., with Mongoose for MongoDB).                              |
| **Middleware** (`/middleware`)           | Custom functions for **auth**, **logging**, **validation**, etc.                             |
| **Views** (`/views`)                     | **Template engine files** (EJS, Pug, Handlebars) for rendering dynamic pages.                |
| **Public** (`/public`)                   | **Static assets** like CSS, JS, images served directly.                                      |

---

## Common Tools & Libraries in Express.js

| **Category**           | **Examples**                                       | **Purpose**                                  |
| ---------------------- | -------------------------------------------------- | -------------------------------------------- |
| **Database Tools**     | **MongoDB**, **MySQL**, **PostgreSQL**             | Store & manage application data.             |
| **ORM/ODM**            | **Mongoose** (MongoDB), **Sequelize** (SQL)        | Simplify DB queries & schema handling.       |
| **Template Engines**   | **EJS**, **Pug**, **Mustache**, **Handlebars**     | Render dynamic HTML pages.                   |
| **Authentication**     | **Passport.js**, **JWT (jsonwebtoken)**, **Auth0** | User authentication & authorization.         |
| **Validation**         | **Joi**, **express-validator**, **Yup**            | Validate request data & enforce schema.      |
| **Logging**            | **Morgan**, **Winston**, **Pino**                  | Log requests, errors, & application events.  |
| **Security**           | **Helmet**, **CORS**, **Rate-limiter**             | Secure apps from common attacks.             |
| **Testing**            | **Jest**, **Mocha**, **Chai**, **Supertest**       | Unit & integration testing.                  |
| **Environment Config** | **dotenv**, **config**                             | Manage environment variables & app settings. |
| **Task Scheduling**    | **node-cron**, **Agenda**                          | Run scheduled jobs (e.g., cron tasks).       |
| **File Uploads**       | **Multer**, **Busboy**                             | Handle file uploads (images, documents).     |
| **API Documentation**  | **Swagger**, **Redoc**                             | Generate & manage API docs.                  |

## What is .env File Used For?

| **Aspect**      | **Description**                                                                            |
| --------------- | ------------------------------------------------------------------------------------------ |
| **Purpose**     | Stores **environment variables** (e.g., API keys, DB credentials) outside the source code. |
| **Security**    | Keeps **sensitive data hidden** from version control (add `.env` to `.gitignore`).         |
| **Usage**       | Loaded using packages like **`dotenv`** (`require('dotenv').config()`).                    |
| **Format**      | Key-value pairs: `PORT=3000`, `DB_URL=mongodb://localhost:27017/app`.                      |
| **Flexibility** | Allows **different configs** for development, testing, and production environments.        |
| **Example**     | `.env` → `PORT=4000` → Access in code: `process.env.PORT`.                                 |

## What is Scaffolding in Express.js?

| **Point**            | **Description**                                                                      |
| -------------------- | ------------------------------------------------------------------------------------ |
| **Definition**       | **Scaffolding** means **auto-generating a project structure** for Express.js apps.   |
| **Purpose**          | Speeds up **initial setup** and maintains **consistent structure**.                  |
| **Tool Used**        | **`express-generator`** (official Express CLI tool).                                 |
| **Command**          | `npm install -g express-generator` (install globally).                               |
| **Create App**       | `express myapp` (creates a new project named `myapp`).                               |
| **Template Options** | Add `--view=ejs`, `--view=pug`, `--view=hbs` to choose a template engine.            |
| **Next Steps**       | Run `cd myapp`, `npm install`, then `npm start` to launch the app.                   |
| **Use Case**         | **Quick setup** for new projects, especially useful for **teams** & **prototyping**. |

## 🛠️ **Installing Node.js & Express**

```bash
# Install Node.js from https://nodejs.org

# Initialize Node project
npm init -y

# Install Express
npm install express
```

## The First "Hello, World" Example

- The first example we're going to create is a simple Express Web Server.

```js
const express = require("express");
const app = express();

app.get("/", (req, res) => res.send("Hello World!"));

app.listen(3000, () => console.log("App is running on port 3000"));
```

---

## 🔁 **What is Routing in Express?**

- **Routing** defines how your application `responds to client requests` to `specific  URLs` using specific `HTTP methods (GET, POST, etc.)`.

- 📍 It links:
  - an `HTTP method`
  - a `URL path`
  - and a `handler function (controller)`

## 🚏 HTTP Methods in Express Routing

| Method | Purpose             | Example                                            |
| ------ | ------------------- | -------------------------------------------------- |
| GET    | Read data           | `app.get('/users',(req, res) => { /* */ })`        |
| POST   | Create new data     | `app.post('/users',(req, res) => { /* */ })`       |
| PUT    | Replace data        | `app.put('/users/:id',(req, res) => { /* */ })`    |
| PATCH  | Update part of data | `app.patch('/users/:id',(req, res) => { /* */ })`  |
| DELETE | Remove data         | `app.delete('/users/:id',(req, res) => { /* */ })` |

## 🔗 **Types of Routes**

| **Type**            | **Description**                                              | **Example**                                                   | **How to Access Values**                |
| ------------------- | ------------------------------------------------------------ | ------------------------------------------------------------- | --------------------------------------- |
| **Static Routes**   | `Fixed URL path`. Same response for every request.           | `app.get('/about', (req, res) => res.send('About'))`          | No dynamic values.                      |
| **Dynamic Routes**  | URLs with parameters (e.g., `:id`) to handle `dynamic data`. | `app.get('/user/:id', (req, res) => res.send(req.params.id))` | `req.params.id`                         |
| **Query Routes**    | Data sent via `URL query string` after `?`.                  | `app.get('/search', (req, res) => res.send(req.query.q))`     | `req.query.q`                           |
| **POST Routes**     | Used to send data in the `body` (JSON, form data).           | `app.post('/user', (req, res) => res.send(req.body.name))`    | `req.body.name` (need `express.json()`) |
| **Nested Routes**   | Routes grouped under a `router` for modular structure.       | `router.get('/profile', (req, res) => res.send('Profile'))`   | Same as normal routes.                  |
| **Wildcard Routes** | Matches any route (used for `404` or `fallback`).            | `app.get('*', (req, res) => res.send('Not Found'))`           | No params, just fallback.               |

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

## `request` (req) Object

- The `req` object `represents the HTTP request` made by the client (browser, API call, etc.).
  It contains `data sent by the client`.

### **Common Properties**

| Property / Method   | Description                               | Example                          |
| ------------------- | ----------------------------------------- | -------------------------------- |
| `req.params`        | Access route parameters                   | `/user/:id` → `req.params.id`    |
| `req.query`         | Access query string parameters            | `/search?q=test` → `req.query.q` |
| `req.body`          | Access data sent in the body (POST/PUT)   | `req.body.name`                  |
| `req.headers`       | Access HTTP request headers               | `req.headers['content-type']`    |
| `req.method`        | Get the HTTP method of the request        | `req.method // GET, POST, etc.`  |
| `req.url`           | Get the full URL of the request           | `req.url`                        |
| `req.path`          | Get the request URL path only             | `req.path`                       |
| `req.protocol`      | Get the protocol used                     | `req.protocol // http or https`  |
| `req.hostname`      | Get the hostname of the request           | `req.hostname`                   |
| `req.ip`            | Get the client’s IP address               | `req.ip`                         |
| `req.get()`         | Get a specific HTTP header                | `req.get('User-Agent')`          |
| `req.cookies`       | Access cookies (requires `cookie-parser`) | `req.cookies.token`              |
| `req.signedCookies` | Access signed cookies                     | `req.signedCookies.user`         |
| `req.secure`        | Check if the connection is HTTPS          | `req.secure // true or false`    |
| `req.xhr`           | Check if request was made via AJAX        | `req.xhr // true or false`       |

---

## `response` (res) Object

The `res` object **represents the HTTP response** that will be sent back to the client.
You **use it to send data, set status codes, and control the response**.

### **Common Methods**

| Method              | Description                                   | Example                                             |
| ------------------- | --------------------------------------------- | --------------------------------------------------- |
| `res.send()`        | Send a response (string, object, etc.)        | `res.send('Hello World')`                           |
| `res.json()`        | Send a JSON response                          | `res.json({ success: true })`                       |
| `res.status()`      | Set HTTP status code                          | `res.status(404).send('Not Found')`                 |
| `res.redirect()`    | Redirect to another URL                       | `res.redirect('/login')`                            |
| `res.render()`      | Render a template (if using a view engine)    | `res.render('home', { title: 'Hi' })`               |
| `res.download()`    | Send a file for download                      | `res.download('file.pdf')`                          |
| `res.sendFile()`    | Send a static file                            | `res.sendFile(__dirname + '/file.pdf')`             |
| `res.set()`         | Set custom response headers                   | `res.set('Content-Type', 'text/plain')`             |
| `res.cookie()`      | Set a cookie in the client browser            | `res.cookie('token', 'abc123', { httpOnly: true })` |
| `res.clearCookie()` | Clear a previously set cookie                 | `res.clearCookie('token')`                          |
| `res.type()`        | Set the Content-Type of the response          | `res.type('application/json')`                      |
| `res.end()`         | End the response process without sending data | `res.end()`                                         |
| `res.append()`      | Append values to HTTP response header         | `res.append('Warning', '199 Misc warning')`         |
| `res.locals`        | Store local variables for templates           | `res.locals.user = { name: 'John' }`                |

## **Example:**

```js
const express = require("express");
const app = express();
app.use(express.json()); // For JSON body parsing

// GET with query & params
app.get("/user/:id", (req, res) => {
  console.log(req.params.id); // Access route parameter
  console.log(req.query.age); // Access query parameter
  res.status(200).json({ userId: req.params.id, age: req.query.age });
});

// POST with body
app.post("/user", (req, res) => {
  console.log(req.body); // Access posted data
  res.cookie("token", "abc123", { httpOnly: true, maxAge: 60000 }); // set cookie
  res.status(201).send(`User ${req.body.name} created!`);
});

app.listen(3000, () => console.log("Server running on port 3000"));
```

---

# 🧠 What is Middleware

- Middleware functions are functions that have access to the `request (req)`, `response (res)`, and `next` function in the request-response cycle.
- They can:
  - Modify `req` or `res`
  - End the request-response cycle
  - Pass control to the next middleware (`next()`)

```js
app.use((req, res, next) => {
  console.log("Middleware executed");
  next(); // Pass control to next middleware/route
});

// Simple user validation middleware
const validateUser = (req, res, next) => {
  const user = req.user;
  if (!user) {
    return res.status(401).json({ error: "Unauthorized - User not found" });
  }
  next();
};

app.get("/profile", validateUser, (req, res) => {
  const user = req.user;
  res.json({ message: "Profile page", username: user.username });
});
```

## 🔁 Middleware Lifecycle:

```txt
Incoming Request → Middleware 1 → Middleware 2 → Route Handler → Response
```

## 🧩 Why Use Middleware?

- ✅ Separation of concerns
- ✅ Reusability (e.g. auth, logging)
- ✅ Centralized logic (e.g. error handling)
- ✅ Cleaner code in route handlers

# 📚 Types of Middleware in Express.js

| **Type**                 | **Description**                                                     | **Example Usage**                         |
| ------------------------ | ------------------------------------------------------------------- | ----------------------------------------- |
| **1. Application-level** | Applied to the entire app using `app.use()` or for specific routes. | Logging, authentication, request parsing. |
| **2. Router-level**      | Applied only to specific routers using `router.use()`.              | Modular routes with their own middleware. |
| **3. Built-in**          | Express provides some by default.                                   | `express.json()`, `express.static()`.     |
| **4. Third-party**       | Installed via npm to add extra features.                            | `morgan` (logging), `cors`, `helmet`.     |
| **5. Error-handling**    | Special middleware to handle errors.                                | Catch and handle errors globally.         |
| **6. Built-in static**   | Serves static files (CSS, images, JS).                              | `express.static('public')`.               |

## 🔹 1. **Application-Level Middleware**

- Used across the entire app using `app.use()` or `app.METHOD()`.
- Runs for every request and logs method + URL.

### ✅ Example: Logging Middleware

```js
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});
```

## 🔹 2. **Router-Level Middleware**

- Attached only to a specific `Router()`.
- Only runs for routes defined in that router.

```js
const router = express.Router();

router.use((req, res, next) => {
  console.log("Middleware for user routes only");
  next();
});
```

## 🔹 2.a. **For specific route Middleware**

- for request data validation : Use `express-validator` or custom validation middleware.

```js
const { body } = require("express-validator");

router.post(
  "/register",
  [body("email").isEmail(), body("password").isLength({ min: 6 })],
  userController.registerUser
);
```

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

## 🔹 5. **Error-Handling Middleware**

- Special signature: **`(err, req, res, next)`**
- This should always be **last** in the middleware chain.

### ✅ Example:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

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

## 🧠 Middleware Order Matters

Middleware is executed **in the order you define it**.

```js
app.use(middleware1);
app.use(middleware2);
app.get("/route", handler);
```

If you place `authMiddleware` **after** the route, it will not run for that route.

## 🔁 Reusable Middleware Pattern

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

## 🔄 What You Can Modify

| Object           | Common Modifications                      |
| ---------------- | ----------------------------------------- |
| `req` (request)  | Add user info, metadata, parsed data      |
| `res` (response) | Set headers, custom response body, status |

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

## 🔴 2. **Modify `res` Object in Middleware**

### ✅ Set response headers:

```js
const addCustomHeader = (req, res, next) => {
  res.setHeader("X-Powered-By", "ShubhamExpressApp");
  next();
};

app.use(addCustomHeader);
```

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

## 🧠 When to Use Templating (in Modern Apps)

- 🖥️ Server-rendered web pages (admin panels, marketing pages)
- ✉️ Dynamic HTML emails
- 🧪 Prototypes with backend-rendered HTML
- 📊 Dashboards or CMS-style tools

---

## 🧾 What is Static File Serving?

In Express.js, **static files** refer to files like:

- `HTML`, `CSS`, `JS`
- Images (`.jpg`, `.png`)
- Fonts, videos, audio
- PDFs or other client-deliverable files

> **Static file serving** means letting the Express server send these files directly to the browser without processing.

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

## 📂 Advanced: Custom URL path for static files

```js
app.use("/static", express.static(path.join(__dirname, "public")));
```

Now access via:

- `http://localhost:3000/static/index.html`

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

### ✅ 2. Middleware to Parse URL-encoded Data

In your `app.js`:

```js
const express = require("express");
const path = require("path");
const app = express();

// Required to parse form data (application/x-www-form-urlencoded)
app.use(express.urlencoded({ extended: true }));

// Handle form submission
app.post("/submit", (req, res) => {
  console.log(req.body); // { username: '...', email: '...' }

  res.send(`Received: ${req.body.username} - ${req.body.email}`);
});

app.listen(3000, () => console.log("Server running on http://localhost:3000"));
```

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

## ✅ Also Handling JSON? Use Both Parsers

```js
app.use(express.json()); // For JSON requests
app.use(express.urlencoded({ extended: true })); // For form submissions
```

---

## **Why Security Matters?**

Express apps are often exposed to attacks like **XSS**, **CSRF**, **SQL Injection**, **Session Hijacking**, etc.
Securing your app is crucial to **protect data & users**.

## **Key Security Practices in Express.js**

### **1. Use Helmet (Set Secure HTTP Headers)**

Helmet helps protect against common attacks by setting security-related headers.

```js
const helmet = require("helmet");
app.use(helmet());
```

### **2. Enable CORS Safely**

Control which domains can access your API.

```js
const cors = require("cors");
app.use(cors({ origin: "https://yourdomain.com" }));
```

### **3. Sanitize User Input**

Prevent **XSS & SQL Injection** by sanitizing input.

```js
const xss = require("xss-clean");
const mongoSanitize = require("express-mongo-sanitize");
app.use(xss());
app.use(mongoSanitize());
```

### **4. Use Rate Limiting**

Prevent **brute-force attacks**.

```js
const rateLimit = require("express-rate-limit");
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100, // Limit each IP to 100 requests
});
app.use(limiter);
```

### **5. Hide Technology Info**

Disable the `X-Powered-By` header to prevent attackers from knowing you use Express.

```js
app.disable("x-powered-by");
```

### **6. Secure Cookies & Sessions**

```js
const session = require("express-session");
app.use(
  session({
    secret: "yourSecretKey",
    cookie: { secure: true, httpOnly: true, sameSite: "strict" },
    resave: false,
    saveUninitialized: false,
  })
);
```

### **7. Use HTTPS**

Always use **HTTPS** in production to encrypt data.

### **8. Prevent CSRF**

Protect against **Cross-Site Request Forgery**.

```js
const csurf = require("csurf");
app.use(csurf());
```

### **9. Validate Input**

Use libraries like **Joi** or **Yup** to validate data.

```js
const Joi = require("joi");
const schema = Joi.object({ email: Joi.string().email().required() });
schema.validate(req.body);
```

### **10. Keep Dependencies Updated**

Regularly run:

```bash
npm audit fix
```

---

## **Why File Handling?**

In Express, file handling is used for:

- Uploading files (images, docs, etc.)
- Serving static files
- Downloading files

---

## **File Handling Methods**

### **A. File Uploading (Using `multer`)**

`multer` is the most popular middleware for handling multipart/form-data.

```js
const express = require("express");
const multer = require("multer");
const app = express();

// Configure storage
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

// Single file upload

// <form action="/single-upload" method="POST" enctype="multipart/form-data">
//   <input type="text" name="username" />
//   <input type="file" name="profile" />
//   <button type="submit">Upload</button>
// </form>

app.post("/single-upload", upload.single("profile"), (req, res) => {
  console.log(req.body); // { username: '...' }
  console.log(req.file); // file info
  res.send("Single file uploaded");
});

// Multiple files upload

// <form action="/multi-upload" method="POST" enctype="multipart/form-data">
//   <input type="text" name="title" />
//   <input type="file" name="photos" multiple />
//   <button type="submit">Upload</button>
// </form>

app.post("/multi-upload", upload.array("photos", 5), (req, res) => {
  console.log(req.body); // { title: '...' }
  console.log(req.files); // array of files
  res.send("Multiple files uploaded");
});

// Text Fields + Multiple Files (Different Field Names)

// <form action="/multi-fields" method="POST" enctype="multipart/form-data">
//   <input type="text" name="username" />
//   <input type="file" name="avatar" />
//   <input type="file" name="resume" />
//   <button type="submit">Upload</button>
// </form>

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

### **B. Serving Static Files**

Serve files like images, CSS, or documents directly:

```js
app.use("/public", express.static("public"));
// Access: http://localhost:3000/public/image.jpg
```

---

### **C. File Download**

Send a file to the client for download:

```js
app.get("/download", (req, res) => {
  res.download(__dirname + "/uploads/sample.pdf");
});
```

---

### **D. Sending Files**

To send files without forcing a download:

```js
app.get("/file", (req, res) => {
  res.sendFile(__dirname + "/uploads/sample.pdf");
});
```

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

# **real-world production-grade Express.js project** with **Node.js + MongoDB**, integrating **security, JWT auth, role-based access, logging, file handling, and performance optimizations**.

## **Project Setup & Folder Structure**

### **Step 1: Initialize the Project**

Run:

```bash
mkdir pro-express-app
cd pro-express-app
npm init -y
```

---

### **Step 2: Install Core Dependencies**

```bash
npm install express mongoose dotenv cors helmet morgan compression jsonwebtoken bcryptjs multer express-validator cookie-parser
```

#### **What & Why**

- **express** – Core server framework
- **mongoose** – MongoDB ODM
- **dotenv** – Load environment variables
- **cors** – Handle Cross-Origin requests
- **helmet** – Security headers
- **morgan** – Logging HTTP requests
- **compression** – Gzip compression for performance
- **jsonwebtoken** – JWT authentication
- **bcryptjs** – Hash passwords securely
- **multer** – File uploads
- **express-validator** – Validation for routes
- **cookie-parser** – Parse cookies

---

### **Step 3: Install Dev Dependencies**

```bash
npm install --save-dev nodemon eslint prettier
```

#### **Why**

- **nodemon** – Auto-restart server on changes
- **eslint & prettier** – Code quality & formatting

---

### **Step 4: Professional Folder Structure**

```
pro-express-app/
│
├── src/
│   ├── config/           # App configs (DB, env, etc.)
│   │   └── db.js
│   │
│   ├── middleware/       # Custom middlewares (auth, errorHandler, etc.)
│   │   ├── auth.js
│   │   ├── errorHandler.js
│   │   └── logger.js
│   │
│   ├── models/           # Mongoose models
│   │   └── User.js
│   │
│   ├── routes/           # Route definitions
│   │   ├── auth.routes.js
│   │   ├── user.routes.js
│   │   └── index.js
│   │
│   ├── controllers/      # Business logic
│   │   ├── auth.controller.js
│   │   └── user.controller.js
│   │
│   ├── services/         # Service layer (DB operations)
│   │   └── user.service.js
│   │
│   ├── utils/            # Helper functions
│   │   ├── generateToken.js
│   │   └── responseHandler.js
│   │
│   ├── uploads/          # File uploads
│   │
│   ├── app.js            # Main Express app setup
│   └── server.js         # Entry point
│
├── .env                  # Environment variables
├── .eslintrc.json        # ESLint config
├── .prettierrc           # Prettier config
├── package.json
└── README.md
```

---

### **Step 5: Environment Variables**

`.env` file:

```env
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/pro_express_app
JWT_SECRET=supersecretkey123
JWT_EXPIRES_IN=7d
```

---

### **Step 6: DB Connection (Mongoose)**

`src/config/db.js`

```js
const mongoose = require("mongoose");

const connectDB = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI);
    console.log("MongoDB Connected");
  } catch (err) {
    console.error("DB Connection Error: ", err.message);
    process.exit(1);
  }
};

module.exports = connectDB;
```

---

### **Step 7: Basic Express App**

`src/app.js`

```js
const express = require("express");
const cors = require("cors");
const helmet = require("helmet");
const morgan = require("morgan");
const compression = require("compression");
const cookieParser = require("cookie-parser");

const routes = require("./routes");
const errorHandler = require("./middleware/errorHandler");

const app = express();

// Middlewares
app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(cors());
app.use(helmet());
app.use(morgan("combined"));
app.use(compression());
app.use(cookieParser());
app.use("/uploads", express.static("src/uploads"));

// Routes
app.use("/api", routes);

// Error handler
app.use(errorHandler);

module.exports = app;
```

---

### **Step 8: Server Entry Point**

`src/server.js`

```js
require("dotenv").config();
const app = require("./app");
const connectDB = require("./config/db");

const PORT = process.env.PORT || 5000;

connectDB();

app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

---

### **Step 9: Global Error Handler**

`src/middleware/errorHandler.js`

```js
module.exports = (err, req, res, next) => {
  console.error(err.stack);
  res.status(err.statusCode || 500).json({
    success: false,
    message: err.message || "Server Error",
  });
};
```

---

### **Step 10: Routes Index**

`src/routes/index.js`

```js
const express = require("express");
const router = express.Router();

const authRoutes = require("./auth.routes");
const userRoutes = require("./user.routes");

router.use("/auth", authRoutes);
router.use("/users", userRoutes);

module.exports = router;
```

---

Great — now let’s **move to Part 2** and start making this a **real-world, production-ready backend** for 2025 standards.

This part will include:

- **User Model** with roles (Admin/User)
- **Register & Login** (JWT-based)
- **Password hashing (bcrypt)**
- **JWT generation & verification**
- **Role-based middleware** (RBAC)
- **Advanced logging with Winston + daily rotate logs**
- **Optimized response handler**

---

## **Auth, Roles, Logging & JWT**

### **1. User Model**

`src/models/User.js`

```js
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const userSchema = new mongoose.Schema(
  {
    name: { type: String, required: true, trim: true },
    email: { type: String, required: true, unique: true, lowercase: true },
    password: { type: String, required: true },
    role: { type: String, enum: ["user", "admin"], default: "user" },
  },
  { timestamps: true }
);

// Hash password before save
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
  next();
});

// Compare password
userSchema.methods.comparePassword = async function (password) {
  return await bcrypt.compare(password, this.password);
};

module.exports = mongoose.model("User", userSchema);
```

---

### **2. JWT Utility**

`src/utils/generateToken.js`

```js
const jwt = require("jsonwebtoken");

const generateToken = (id, role) => {
  return jwt.sign({ id, role }, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRES_IN || "7d",
  });
};

module.exports = generateToken;
```

---

### **3. Response Handler Utility**

`src/utils/responseHandler.js`

```js
module.exports = (res, statusCode, success, message, data = {}) => {
  res.status(statusCode).json({
    success,
    message,
    data,
  });
};
```

---

### **4. Auth Controller**

`src/controllers/auth.controller.js`

```js
const User = require("../models/User");
const generateToken = require("../utils/generateToken");
const responseHandler = require("../utils/responseHandler");

exports.register = async (req, res, next) => {
  try {
    const { name, email, password, role } = req.body;

    if (!name || !email || !password) {
      return responseHandler(res, 400, false, "All fields are required");
    }

    const existingUser = await User.findOne({ email });
    if (existingUser) {
      return responseHandler(res, 400, false, "User already exists");
    }

    const user = await User.create({ name, email, password, role });
    const token = generateToken(user._id, user.role);

    return responseHandler(res, 201, true, "User registered successfully", {
      token,
      user: {
        id: user._id,
        name: user.name,
        email: user.email,
        role: user.role,
      },
    });
  } catch (err) {
    next(err);
  }
};

exports.login = async (req, res, next) => {
  try {
    const { email, password } = req.body;

    if (!email || !password) {
      return responseHandler(res, 400, false, "All fields are required");
    }

    const user = await User.findOne({ email });
    if (!user) {
      return responseHandler(res, 401, false, "Invalid credentials");
    }

    const isMatch = await user.comparePassword(password);
    if (!isMatch) {
      return responseHandler(res, 401, false, "Invalid credentials");
    }

    const token = generateToken(user._id, user.role);
    return responseHandler(res, 200, true, "Login successful", {
      token,
      user: {
        id: user._id,
        name: user.name,
        email: user.email,
        role: user.role,
      },
    });
  } catch (err) {
    next(err);
  }
};
```

---

### **5. Auth Routes**

`src/routes/auth.routes.js`

```js
const express = require("express");
const { register, login } = require("../controllers/auth.controller");
const { body } = require("express-validator");

const router = express.Router();

router.post(
  "/register",
  [
    body("name").notEmpty().withMessage("Name is required"),
    body("email").isEmail().withMessage("Valid email is required"),
    body("password")
      .isLength({ min: 6 })
      .withMessage("Password must be at least 6 characters"),
  ],
  register
);

router.post(
  "/login",
  [
    body("email").isEmail().withMessage("Valid email is required"),
    body("password").notEmpty().withMessage("Password is required"),
  ],
  login
);

module.exports = router;
```

---

### **6. Auth Middleware (JWT Verification)**

`src/middleware/auth.js`

```js
const jwt = require("jsonwebtoken");
const responseHandler = require("../utils/responseHandler");

exports.protect = (req, res, next) => {
  let token =
    req.headers.authorization && req.headers.authorization.startsWith("Bearer")
      ? req.headers.authorization.split(" ")[1]
      : null;

  if (!token)
    return responseHandler(res, 401, false, "Not authorized, no token");

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded; // { id, role }
    next();
  } catch (err) {
    return responseHandler(res, 401, false, "Token is not valid");
  }
};

exports.authorize = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return responseHandler(res, 403, false, "You do not have permission");
    }
    next();
  };
};
```

---

### **7. Advanced Logging with Winston**

`src/middleware/logger.js`

```js
const { createLogger, transports, format } = require("winston");
require("winston-daily-rotate-file");

const logger = createLogger({
  format: format.combine(
    format.timestamp({ format: "YYYY-MM-DD HH:mm:ss" }),
    format.printf(
      (info) =>
        `${info.timestamp} [${info.level.toUpperCase()}]: ${info.message}`
    )
  ),
  transports: [
    new transports.Console(),
    new transports.DailyRotateFile({
      filename: "logs/app-%DATE%.log",
      datePattern: "YYYY-MM-DD",
      zippedArchive: true,
      maxSize: "20m",
      maxFiles: "14d",
    }),
  ],
});

module.exports = logger;
```

In `src/app.js`:

```js
const logger = require("./middleware/logger");
app.use((req, res, next) => {
  logger.info(`${req.method} ${req.originalUrl}`);
  next();
});
```

---

### **8. Example User Route (Protected & Role-based)**

`src/routes/user.routes.js`

```js
const express = require("express");
const { protect, authorize } = require("../middleware/auth");
const responseHandler = require("../utils/responseHandler");

const router = express.Router();

router.get("/me", protect, (req, res) => {
  return responseHandler(res, 200, true, "User profile fetched", req.user);
});

router.get("/admin", protect, authorize("admin"), (req, res) => {
  return responseHandler(res, 200, true, "Welcome Admin");
});

module.exports = router;
```

---

Perfect — let’s move to **Part 3** and make our backend **even more production-grade** without Redis.

This part will include:

- **Secure File Uploads (Multer)**
- **Rate Limiting & Brute-force Protection**
- **Request Sanitization (XSS & NoSQL injection)**
- **Pagination & Filtering for APIs**
- **Basic Performance Tweaks**

---

## **File Uploads, Security Enhancements & API Improvements**

---

### **1. Secure File Upload Handling**

We’ll use **Multer** for image/file uploads with **validation**.

`src/middleware/fileUpload.js`

```js
const multer = require("multer");
const path = require("path");

const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, "src/uploads/");
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + "-" + file.originalname);
  },
});

const fileFilter = (req, file, cb) => {
  const allowedTypes = /jpeg|jpg|png|pdf/;
  const extname = allowedTypes.test(
    path.extname(file.originalname).toLowerCase()
  );
  const mimetype = allowedTypes.test(file.mimetype);
  if (extname && mimetype) {
    cb(null, true);
  } else {
    cb(new Error("Only images and PDFs are allowed"));
  }
};

const upload = multer({
  storage,
  fileFilter,
  limits: { fileSize: 2 * 1024 * 1024 },
}); // 2MB limit

module.exports = upload;
```

---

### **2. Example File Upload Route**

`src/routes/user.routes.js` (Add endpoint)

```js
const upload = require("../middleware/fileUpload");

router.post("/upload", protect, upload.single("file"), (req, res) => {
  if (!req.file) return responseHandler(res, 400, false, "No file uploaded");
  return responseHandler(res, 200, true, "File uploaded successfully", {
    file: req.file.filename,
  });
});
```

---

### **3. Rate Limiting & Brute-force Protection**

We’ll use **express-rate-limit** to prevent excessive requests.

```bash
npm install express-rate-limit
```

`src/middleware/rateLimiter.js`

```js
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 mins
  max: 100, // limit each IP to 100 requests per window
  message: { success: false, message: "Too many requests, try again later" },
});

module.exports = limiter;
```

In `src/app.js`:

```js
const limiter = require("./middleware/rateLimiter");
app.use(limiter);
```

---

### **4. Prevent XSS & NoSQL Injection**

Install:

```bash
npm install xss-clean express-mongo-sanitize
```

In `src/app.js`:

```js
const xss = require("xss-clean");
const mongoSanitize = require("express-mongo-sanitize");

app.use(xss()); // Prevent XSS
app.use(mongoSanitize()); // Prevent NoSQL injection
```

---

### **5. Pagination & Filtering**

Reusable utility for APIs.

`src/utils/apiFeatures.js`

```js
class APIFeatures {
  constructor(query, queryString) {
    this.query = query;
    this.queryString = queryString;
  }

  filter() {
    const queryObj = { ...this.queryString };
    const excludeFields = ["page", "sort", "limit"];
    excludeFields.forEach((el) => delete queryObj[el]);

    this.query = this.query.find(queryObj);
    return this;
  }

  sort() {
    if (this.queryString.sort) {
      this.query = this.query.sort(this.queryString.sort.split(",").join(" "));
    } else {
      this.query = this.query.sort("-createdAt");
    }
    return this;
  }

  paginate() {
    const page = parseInt(this.queryString.page, 10) || 1;
    const limit = parseInt(this.queryString.limit, 10) || 10;
    const skip = (page - 1) * limit;
    this.query = this.query.skip(skip).limit(limit);
    return this;
  }
}

module.exports = APIFeatures;
```

---

### **6. Example: Paginated Users API**

`src/controllers/user.controller.js`

```js
const User = require("../models/User");
const APIFeatures = require("../utils/apiFeatures");
const responseHandler = require("../utils/responseHandler");

exports.getUsers = async (req, res, next) => {
  try {
    const features = new APIFeatures(User.find(), req.query)
      .filter()
      .sort()
      .paginate();
    const users = await features.query;

    return responseHandler(res, 200, true, "Users fetched successfully", users);
  } catch (err) {
    next(err);
  }
};
```

`src/routes/user.routes.js`

```js
const { getUsers } = require("../controllers/user.controller");
router.get("/", protect, authorize("admin"), getUsers);
```

---

### **7. Performance Tweaks**

- **Compression** already added (gzip).
- **Indexes for frequently queried fields**:

```js
userSchema.index({ email: 1 });
userSchema.index({ role: 1 });
```

- **Lean queries for read-heavy endpoints** (e.g., `User.find().lean()` for better performance).
- **Avoid N+1 queries** (populate selectively).

---

## What is .env File Used For?

| **Aspect**      | **Description**                                                                            |
| --------------- | ------------------------------------------------------------------------------------------ |
| **Purpose**     | Stores **environment variables** (e.g., API keys, DB credentials) outside the source code. |
| **Security**    | Keeps **sensitive data hidden** from version control (add `.env` to `.gitignore`).         |
| **Usage**       | Loaded using packages like **`dotenv`** (`require('dotenv').config()`).                    |
| **Format**      | Key-value pairs: `PORT=3000`, `DB_URL=mongodb://localhost:27017/app`.                      |
| **Flexibility** | Allows **different configs** for development, testing, and production environments.        |
| **Example**     | `.env` → `PORT=4000` → Access in code: `process.env.PORT`.                                 |

## What are JWT?

| **Aspect**    | **Description**                                                                                      |
| ------------- | ---------------------------------------------------------------------------------------------------- |
| **Full Form** | **JSON Web Token**                                                                                   |
| **Purpose**   | Used for **securely transmitting information** (like authentication data) between client and server. |
| **Structure** | Consists of **3 parts**: `Header`.`Payload`.`Signature`.                                             |
| **Header**    | Contains **algorithm & token type** (e.g., HS256, JWT).                                              |
| **Payload**   | Holds **data (claims)** like user ID, roles, expiry time.                                            |
| **Signature** | Verifies that the token **hasn’t been tampered with** (created using a secret key).                  |
| **Use Cases** | **Authentication**, **authorization**, API security (e.g., login sessions without cookies).          |
| **Storage**   | Typically stored in **localStorage**, **sessionStorage**, or **HTTP-only cookies**.                  |
| **Stateless** | Server doesn’t store session data — token itself carries info.                                       |

## What is Bcrypt Used For?

| **Aspect**          | **Description**                                                                                                   |
| ------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Purpose**         | **Hashing passwords** to store them securely in the database.                                                     |
| **Why Use It?**     | Prevents storing **plain-text passwords**; makes it hard for attackers to retrieve original passwords.            |
| **Salting**         | Adds a **random string (salt)** to the password before hashing, making it resistant to **rainbow table attacks**. |
| **One-way Hashing** | Passwords **cannot be reversed** back to their original form.                                                     |
| **Use Cases**       | **User authentication systems**, API security, login/signup flows.                                                |
| **Common Methods**  | `bcrypt.hash()` (to hash passwords), `bcrypt.compare()` (to verify passwords).                                    |

## What is ESLint?

| **Aspect**          | **Description**                                                                              |
| ------------------- | -------------------------------------------------------------------------------------------- |
| **Purpose**         | A **linting tool** for JavaScript/Node.js that helps **find and fix code issues**.           |
| **Why Use It?**     | Ensures **code quality**, **consistency**, and catches **errors early**.                     |
| **Customizable**    | Supports **custom rules** & **predefined configs** (e.g., Airbnb, Google).                   |
| **Integration**     | Works with **VS Code**, **CI/CD pipelines**, and build tools.                                |
| **Common Commands** | `eslint .` (lint all files), `eslint --fix` (auto-fix issues).                               |
| **Use Cases**       | Enforcing **coding standards**, **detecting bugs**, and **maintaining clean code** in teams. |

## What is CORS in Express.js?

| **Aspect**           | **Description**                                                                                                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| **Full Form**        | **Cross-Origin Resource Sharing**                                                                                      |
| **Purpose**          | Allows a server to **control which domains** can access its resources (API, files, etc.).                              |
| **Why Needed?**      | **Browsers block requests** from different origins (domains, ports, protocols) for security. CORS relaxes this policy. |
| **Usage in Express** | Implemented using the **`cors` middleware** (`npm install cors`).                                                      |
| **Basic Setup**      | `app.use(require('cors')())` (enables CORS for all routes).                                                            |
| **Advanced Config**  | Specify **allowed origins, methods, headers, credentials**.                                                            |
| **Use Cases**        | **APIs accessed by frontends** hosted on different domains (e.g., React app calling Express API).                      |

## What is Sanitizing Input in Express.js?

| **Aspect**           | **Description**                                                                                                      |
| -------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Definition**       | **Cleaning user inputs** to remove **malicious or unwanted data** before processing/storing them.                    |
| **Purpose**          | Prevents **security attacks** like **XSS (Cross-Site Scripting)**, **SQL Injection**, and **HTML/script injection**. |
| **Common Libraries** | **express-validator**, **DOMPurify**, **sanitize-html**, **xss-clean**.                                              |
| **Examples**         | Removing `<script>` tags, stripping special characters, or validating formats (email, phone).                        |
| **Usage in Express** | Often used as **middleware**: `app.post('/form', sanitizeMiddleware, handler)`.                                      |
| **Best Practice**    | Always **validate & sanitize** user input before using it in DB queries or rendering on pages.                       |
