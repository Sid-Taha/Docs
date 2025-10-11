## 🧩 **1. Introduction to APIs**

### What is an API?

**API (Application Programming Interface)** is a bridge that allows two software systems to communicate.
When you send a request to a server, and it responds with data — that’s an API in action.

For example:

```js
app.get("/books", (req, res) => res.json(books))
```
---

### Types of APIs


#### 🟢 **Public API**

* Available for everyone on the internet.
* Example: OpenWeatherMap API, GitHub API.
* Used for integration and innovation.
* Usually rate-limited and may need API keys.

For example:  

```js
app.get("/public", (req, res) => res.json({ msg: "Anyone can access this" }))
```

---


#### 🔒 **Private API**

* Used internally within a company.
* Example: Facebook’s internal microservices.
* Secured behind firewalls and not exposed publicly.

For example:

```js
app.get("/private", (req, res) => {
  const key = req.headers["x-api-key"]
  if (key === "12345") res.json({ msg: "Welcome authorized user" })
  else res.status(403).json({ error: "Access Denied" })
})
```
---

#### 🤝 **Partner API**

* Shared between business partners under contracts.
* Example: Uber exposes partner APIs for restaurants.
* Requires authentication and approval.

For example:

```js
app.get("/partner", (req, res) => {
  const partner = req.headers["x-partner-id"]
  if (partner === "ASTOLIXGEN") res.json({ msg: "Hello Partner" })
  else res.status(401).json({ error: "Invalid Partner" })
})
```

---

## ⚙️ **2. API Terminologies**

### 🔸 HTTP Versions

* **HTTP/1.1** – Most common; requests and responses are text-based.
* **HTTP/2** – Binary, multiplexing (parallel requests).
* **HTTP/3 (QUIC)** – Uses UDP for faster, reliable transport.

### 🔸 HTTP Methods

| Method | Purpose             | Example        |
| ------ | ------------------- | -------------- |
| GET    | Retrieve data       | `/api/users`   |
| POST   | Create data         | `/api/users`   |
| PUT    | Replace entire data | `/api/users/1` |
| PATCH  | Update part of data | `/api/users/1` |
| DELETE | Remove data         | `/api/users/1` |


For example:  

Your **/books** has a `GET`,`POST`, `PUT`, and `DELETE` methods:

```js
app.use(express.json())

// POST - create a new book
app.post("/books", (req, res) => {
  const newBook = req.body
  books.push(newBook)
  res.status(201).json(newBook)
})

// PUT - replace book
app.put("/books/:id", (req, res) => {
  const id = parseInt(req.params.id)
  const index = books.findIndex(b => b.id === id)
  if (index === -1) return res.status(404).json({ error: "Book not found" })
  books[index] = req.body
  res.json(books[index])
})

// DELETE - remove a book
app.delete("/books/:id", (req, res) => {
  const id = parseInt(req.params.id)
  const index = books.findIndex(b => b.id === id)
  if (index === -1) return res.status(404).json({ error: "Book not found" })
  books.splice(index, 1)
  res.status(204).send()
})
```

### 📡 HTTP Status

| Code | Meaning               | Example                     |
| ---- | --------------------- | --------------------------- |
| 200  | OK                    | Successful request          |
| 201  | Created               | Resource created            |
| 400  | Bad Request           | Invalid data                |
| 401  | Unauthorized          | Missing/invalid credentials |
| 404  | Not Found             | Resource doesn’t exist      |
| 500  | Internal Server Error | Server issue                |

### 📡 HTTP Headers


Headers send **metadata** along with requests/responses.
Examples:

* `Content-Type: application/json`
* `Authorization: Bearer <token>`
* `Cache-Control: no-cache`

For example:

```js
app.get("/headers", (req, res) => {
  res.setHeader("X-Powered-By", "Taha Express API")
  res.status(200).json({ msg: "Header set successfully" })
})
```

### 🍪 Cookies

* Small data pieces stored in the browser.
* Used for sessions and authentication.
* Example: `Set-Cookie: sessionId=abc123; HttpOnly`

For Example:

```js
const cookieParser = require("cookie-parser")
app.use(cookieParser())

app.get("/set-cookie", (req, res) => {
  res.cookie("user", "Taha", { httpOnly: true })
  res.send("Cookie set!")
})

app.get("/read-cookie", (req, res) => {
  res.json({ cookie: req.cookies.user })
})
```  

### 🧠 Caching Example

* Improves performance by storing frequent responses.
* Example: CDN caching, `Cache-Control`, `ETag`.

For Example:

```js
app.get("/cached", (req, res) => {
  res.set("Cache-Control", "public, max-age=60") // 60s cache
  res.json({ data: "This response is cached for 1 minute" })
})
```

---

## 🧱 3. API Style – REST API

### What is REST?

**REST (Representational State Transfer)** is an architectural style for designing APIs.
It uses:

* HTTP methods properly (`GET`, `POST`, etc.)
* JSON as the format
* Stateless design (no session stored on server)

For Example:

```js
app.get("/api/v1/books", (req, res) => {
  res.status(200).json({ success: true, data: books })
})
```

### REST Best Practices

* Use **nouns**, not verbs: `/users` not `/getUsers`
* Use **plural names** for collections.
* Handle errors with proper **status codes**.
* Support **filtering and pagination**.
* Always use **HTTPS**.

---

## 🔐 4. **API Authentication**

### 🔸 Basic Authentication

* Username and password encoded in Base64.
* Example:

  ```
  Authorization: Basic dXNlcjpwYXNz
  ```


### 🧾 Basic Auth

For Example:
```js
app.get("/basic-auth", (req, res) => {
  const auth = req.headers.authorization
  if (!auth) return res.status(401).send("Missing auth")
  const [type, credentials] = auth.split(" ")
  const [user, pass] = Buffer.from(credentials, "base64").toString().split(":")
  if (user === "taha" && pass === "123") res.send("Welcome Taha")
  else res.status(403).send("Invalid credentials")
})
```
---
### 🔸 Token Authentication

* Server issues a token; client uses it in headers.

  ```
  Authorization: Token abc123xyz
  ```

### 🔑 Token Auth
For Example:

```js
app.get("/token", (req, res) => {
  const token = req.headers["x-access-token"]
  if (token === "secret123") res.json({ msg: "Valid token" })
  else res.status(401).json({ error: "Invalid token" })
})
```
---

### 🔸 JWT (JSON Web Token)

* Compact, signed token (Base64 + Signature).
* Structure: `Header.Payload.Signature`
* Example:

  ```
  Authorization: Bearer <JWT>
  ```
* Used widely in Express.js with `jsonwebtoken`.

### 🧩 JWT Auth (using `jsonwebtoken`)
For Example:
```bash
npm i jsonwebtoken
```

```js
const jwt = require("jsonwebtoken")
const SECRET = "mysecret"

app.post("/login", (req, res) => {
  const { username } = req.body
  const token = jwt.sign({ username }, SECRET, { expiresIn: "1h" })
  res.json({ token })
})

app.get("/profile", (req, res) => {
  const token = req.headers.authorization?.split(" ")[1]
  try {
    const decoded = jwt.verify(token, SECRET)
    res.json({ msg: `Welcome ${decoded.username}` })
  } catch (e) {
    res.status(401).json({ error: "Invalid Token" })
  }
})
```
---

### 🔸 OAuth 2.0

* Delegated authorization (used by Google, GitHub).
* Flow: User → Authorization Server → Token → API Access.

### 🔸 Session Authentication

* Server creates a session (ID stored in cookie).
* Common in traditional web apps.

---

## 📘 5. API Documentation Tools

### 🔹 Postman

* GUI tool to **test and document APIs**.
* You can save requests, share collections, and run automated tests.  
* You can test your API routes (GET/POST/PUT/DELETE) here.


### 🔹 Swagger (OpenAPI)

* Generates interactive documentation automatically.
* Example:

  ```python
  @app.get("/users")
  def get_users():
      """Get list of all users"""
  ```

  Swagger UI shows this as clickable API.

---

## ⚡ 6. API Features

### 🔸 Pagination

Used to split large data results into pages.

Example:

```
GET /api/users?page=2&limit=10
```

```js
app.get("/paged-books", (req, res) => {
  const { page = 1, limit = 2 } = req.query
  const start = (page - 1) * limit
  const end = page * limit
  res.json(books.slice(start, end))
})
```
---

### 🔸 Idempotency

A request that can be safely retried without changing state.


* `GET`, `PUT`, `DELETE` should be idempotent.

* `PUT` and `DELETE` are idempotent (same request → same result).

For Example:  

```js
app.get("/search", (req, res) => {
  const { author } = req.query
  const result = books.filter(b => b.author.includes(author))
  res.json(result)
})
```
---

### 🔸 HATEOAS

Hypermedia as the Engine of Application State.
Server sends links for next possible actions.

For Example: 
```json
{
  "id": 1,
  "name": "Book",
  "links": [{ "rel": "self", "href": "/api/books/1" }]
}
```

---

### 🔸 URL, Query, Path Parameters

| Type  | Example         | Description                             |
| ----- | --------------- | --------------------------------------- |
| Path  | `/users/123`    | Identifies resource                     |
| Query | `/users?age=20` | Filters                                 |
| URL   | Full address    | `https://api.com/users/123?active=true` |

---

### 🔸 API Versioning

To avoid breaking clients:

```
/api/v1/users
/api/v2/users
```
For Example: 

```js
app.get("/api/v2/books", (req, res) => {
  res.json({ version: "v2", books })
})
```
---

### 🔸 Content Negotiation

Client requests specific format:

```
Accept: application/json
Accept: application/xml
```

---

## 🚀 7. API Performance

### 🔹 Caching

Store repeated responses using:

* Redis
* CDN
* HTTP headers

---

### 🔹 Rate Limiting

Prevents abuse by limiting requests per minute.
Libraries: `express-rate-limit`

```bash
npm i express-rate-limit
```

```js
const rateLimit = require("express-rate-limit")
const limiter = rateLimit({ windowMs: 60 * 1000, max: 5 }) // 5 requests/min
app.use("/books", limiter)
```

---

### 🔹 Load Balancing

Distributes load across multiple servers.
Example: Nginx, AWS ELB.

---

### 🔹 Pagination

Avoid fetching all data at once.

---

### 🔹 Indexing

Database indexes make queries faster.

Handled in **database** (MongoDB indexes, etc.) and cloud scaling (AWS EC2, Docker).

---

### 🔹 Scaling

Use horizontal scaling (multiple servers) and vertical (more CPU/RAM).

---

### 🔹 Performance Testing

Use tools like:

* **Apache JMeter**
* **k6**
* **Artillery**

---

## 🧰 8. API Gateways

Gateways manage, secure, and scale APIs.

| Gateway                  | Description                            |
| ------------------------ | -------------------------------------- |
| **AWS API Gateway**      | Serverless API management on AWS       |
| **Kong**                 | Open-source gateway with plugins       |
| **Azure API Management** | Cloud-based gateway on Azure           |
| **Apigee (Google)**      | Enterprise API platform with analytics |
| **Nginx**                | Reverse proxy and load balancer        |

You can deploy this Express app behind:

* **Nginx** (reverse proxy)
* **Kong** (plugin-based)
* **AWS API Gateway** (serverless)

Example (conceptual):

```

Client → API Gateway → Express App → Database
```


---

## 🔄 9. API Integration Patterns

### 🔸 Sync vs Async

* **Synchronous** – Immediate response (REST).
* **Asynchronous** – Delayed, event-based (WebSockets, queues).

---

### 🔸 API Gateway

Central entry point for routing, security, and monitoring.

---

### 🔸 Microservices

Breaking app into independent small services that talk via APIs.

---

### 🔸 Webhooks

Server sends **real-time updates** to your endpoint.
Example: Stripe sends payment event → your server processes it.

### Webhook Example

```js
app.post("/webhook/payment", (req, res) => {
  console.log("Payment event:", req.body)
  res.status(200).send("Received")
})
```

---

### 🔸 Polling

Client keeps checking for updates:

```
GET /api/status/123 every 10s
```

```js
app.get("/status/:id", (req, res) => {
  res.json({ id: req.params.id, status: "Processing" })
})
```

---

### 🔸 Batch Processing

Send bulk data at once instead of multiple calls.

---

### 🔸 Message Queue

Used for async communication.
Example: RabbitMQ, Kafka, Redis Streams.

## 🧠 10. **Summary**

| Topic       | Example              |
| ----------- | -------------------- |
| Public API  | `/public`            |
| Private API | `/private`           |
| Auth        | `/login` with JWT    |
| Pagination  | `/paged-books`       |
| Rate Limit  | 5 requests/min       |
| Docs        | `/docs` with Swagger |

---
