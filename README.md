## Most Popular NodeJs Interview Questions and Answers

👋 Hello Developers!

**Welcome to my GitHub repository!** 🙏

I’ve created a curated collection of important Nodejs interview questions and answers designed specifically for biginner and experienced developers.

These questions cover core fundamentals, advanced concepts, real-world examples, and deep explanations across the entire NodeJS.

**If you go through these Q&A sets, you will:**

✔ Improve your NodeJs knowledge

✔ Prepare confidently for advanced-level interviews

✔ Understand Nodejs behavior with clear examples

---



## 📌 1. What is Node.js?

Node.js is an open-source, cross-platform JavaScript runtime environment used to execute JS outside the browser.
It uses Google Chrome’s V8 engine to run JavaScript at high performance.

### Key Features

🔸Single Threaded
🔸Asynchronous & Non-Blocking I/O
🔸Event-Driven Architecture
🔸Scalable & High Performance
🔸Perfect for APIs, real-time apps, microservices.
```js
```

---

## 📌 2. What is NPM?
NPM (Node Package Manager) manages dependencies for Node applications.

### ✳ What it does:
Installs packages, Manages versions, Auto-updates dependencies, Uses package.json

### ✳ Commands:
```js
npm install package-name
npm uninstall package-name

```
---

## 📌 3. Why Node.js is Single-Threaded?

Node.js uses a single execution thread because:
It is designed for non-blocking asynchronous operations
It uses callbacks, promises, event loop.

---

## 📌 4. What is Cluster?

The Cluster module in Node.js allows you to run multiple instances (workers) of your application so that you can utilize all CPU cores of your system.
By default, Node.js runs on a single thread, which means only one CPU core is used—even if your system has 4, 8, or 12 cores.

### 🔥 Why Cluster is Used?
It helps to:
🔸Increase application performance

🔸Improve scalability

🔸Handle high load smoothly

🔸Serve multiple requests in parallel

### 🚀 How Cluster Works?
When you enable clustering:
1. Node.js creates a master process.
2. The master process detects the total number of CPU cores (e.g., 12 cores).
3. Based on this number, Node.js creates multiple worker processes (usually equal to CPU cores).
4. Each worker is an individual Node/Express server.
5. All workers share the same port (e.g., 3000) and handle requests independently.
So, if your system has 12 CPU cores, the cluster module can create:12 separate Express servers

🧠 Simple Example

### Without cluster
➡ Only 1 server running
➡ Only 1 CPU core used
➡ Application gets slow under heavy load

### With cluster
➡ If you have 12 cores → 12 workers
➡ 12 servers run in parallel
➡ Your application handles many more requests smoothly

### Real-Life Example: Using Cluster in a Node.js + Express Project
We will create:
server.js → Main server (Express app)
cluster.js → Cluster manager (handles multiple CPUs)
This is the best and most common structure for production.

### 📌 Step 1: Create server.js (Your Express App)

```js
// server.js
const express = require("express");
const app = express();

// Simple API endpoint
app.get('/', (req, res)=>{
res.send(`Server running on process : ${process.id}`);

});

// Heavy CPU task simulation (to see cluster benefit)
app.get("/heavy", (req, res) => {
  let sum = 0;
  for (let i = 0; i < 5000000000; i++) {
    sum += i;
  }
  res.send(`Heavy task done by PID: ${process.pid}`);
});

// Start server
module.exports = app;

```


### 📌 Step 2: Create cluster.js (Cluster Implementation)


```js
// cluster.js
const cluster = require("cluster");
const os = require("os");

if(cluster.isMaster) {
 console.log(`Master started: PID = ${process.pid}`);
 const cpuCount = os.cpus().length;
 console.log(`Total CPU Cores = ${cpuCount}`);
 console.log("Starting workers...\n");

 // Create workers equal to CPU cores
  for (let i = 0; i < cpuCount; i++) {
    cluster.fork();
  }

  // Restart worker if it crashes
  cluster.on("exit", (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died. Starting new worker...`);
    cluster.fork();
  });

} else {

 const app = require("./server");
 app.listen(3000, () => {
    console.log(`Worker running: PID = ${process.pid}`);
  });
}

```

### 📌 Step 3: Run Your Cluster Server

```js
node cluster.js

```

### 🎉 What Happens Now?
If your system has 8 CPU cores, Node.js will create:
🔸1 Master process
🔸8 Worker processes (each one running the same Express server)

You will see output like:

```js
Master started: PID = 1200
Total CPU Cores = 8
Starting workers...

Worker running: PID = 1210
Worker running: PID = 1211
Worker running: PID = 1212

```

### 🔍 Test the Load Balancing
Open a browser and hit: http://localhost:3000/
Refresh multiple times.
You will see different PID numbers, meaning requests are served by different workers.

In the Production Server PM2 supports clustering automatically:

```js

pm2 start server.js -i max

```


---


## 📌 5. How to Improve Node Js API Performance?

For improve API performance we need to work over 3 things.

### 1. Code-Level Optimization
🔸Use Async/Await & Non-Blocking Code. 

🔸Avoid heavy operations inside loops.

🔸Use efficient array methods like (map, reduce).

🔸Use Pagination for handle the large data.

🔸Use Cacheing (Radis) for Cache repeated queries to reduce database load.

🔸Optimize Middleware : Unnecessary middleware slows Node.js app.
    Avoid too many app.use()

    Don’t use bodyParser for all routes

    Don’t log everything in production

    Use route-specific middleware

    Reduce JSON Response Size 

🔸 We can use Load Testing (JMeter) and identify the slow API endpoint and optimize them

🔸 Optimize Environment : Disable debug logs in production

🔸 Throttling & Rate Limiting : Prevent server overload using express-rate-limit or Nginx rate limit.

### 2. Database-Level Optimization
Database contributes to nearly 70% of API performance.

🔹 Use Proper indexes on frequently filtered columns.

🔹 Optimize Query Structure : Avoid Select *, Unnecessary Joins, 

🔹 Use Replication & Read/Write Splitting.
   
🔹 Use Joins Carefully.

🔹 We can implement sharding for Big Data : It is a distrubeted System splitting one large database into many smaller databases so the system becomes faster.

🔹 Choose the Right DB

For relational data → We can go with MySQL/PostgreSQL.

For large writable data → We can go with MongoDB. 

For large-scale search → We can go with Elasticsearch.





### 3. Infrastructure-Level Optimization
    
    These improve scalability and handle high traffic.

🔸 Use load Balancer (Nginx) : Distributes load .

🔸 Use PM2 with Cluster (Horizontal Scaling) : Use all CPU cores and run multiple Node.js instances.

🔸 Use CDN for improve static asset delivery (images, video, pdf, document,fonts, css, js )

🔸 Use connection pooling : Avoids too many connections and speeds up db queries.

🔸 Enable Compression (Gzip) : Compress responses at infrastructure level

🔸 Use Environment-Specific Builds: Production build with Minified code, Disabled debugging



---

## 📌 6. What is CDN?
CDN = Content Delivery Network
A global network of servers that deliver content faster.
A CDN  is a network of servers located in different countries. Its job is to deliver your website files (images, videos, CSS, JS, etc.) faster to users. 

### 🟧 Why do we need CDN?
Because without CDN:
 All users download files from your single server and it is slow.
 If the server is not close to the user, the website will load slower
 Example:
      Your server is in India, but a user is in USA. The distance is big , then website loads slow. AND CDN solves this.

### 🟧 How CDN Works? (Super Easy)
1. Your main server → Original files.
2. CDN → Copies of your files and stored around the world.
3. User → Gets the file from the closest CDN server.

### Popular CDNs
Cloudflare , Akamai, AWS CloudFront, Google CDN, Fastly


---

## 📌 7. What is Middleware?
Middleware is a mechanism that is use to filter the http request in the application.
And in the application multiple types of middle ware.
1. Application level middleware - → app.use()
2. Route level middleware → router.use()
3. Built-in Middle → express.json(), express.static()
4. Third-party : cors, body-parser, helmet, morgan

---

### 📌 8. What is Body Parser?
Body-parser reads and parses incoming HTTP request bodies for POST, PUT, PATCH.

---

### 📌 9. What is CORS?
CORS (Cross-Origin Resource Sharing) allows requests from different domains.


---

### 📌 10. How to create a DB connection and  complete API in node js using async/await ?

A scalable Node.js app uses a singleton database connection file.
## MySQL Example
```js
require('dotenv').config();
const mysql = require('mysql2');
let pool;
cosnt getConnection = ()=>{
   if(!pool){
      pool = mysql.createPool({
        host : process.env.MYSQL_HOST,
        user : process.env.MYSQL_USER,
        password : process.env.MYSQL_PASSWORD,
        database : process.env.MYSQL_DB,
        connectionLimit : process.env.MYSQL_CONNECTION_LIMIT
      });

    }

    return pool;
}

module.exports = getConnection();
```
## Service Layer

```js
const db = require('../connection/db');
const getStudents = async()=>{
    const [rows] = await db.promise().query("SELECT id,name,mobile,emial from students");
    return rows;
};

module.exports = {getStudent};

```

## Controller

```js
const studentService = require('../service/studentService');
const getStudent = async(req, res)=>{
     try{
      cosnt students = await studentService.getStudents();
      res.json({
        success :true,
        result : students
      });
     }catch(error){
     
     res.status(500).json({
        success: false,
        message : 'server error'
     })

     }
}

module.exports = {getStudent};
```

###  Route
```js
const express = require('express');
const router = express.Router();
const studentController = require('../controller/studentController');

router.get('/list', studentController.getStudents);
module.exports = router;

```
---

###  📌 11. What is .env?
It is a environmental file and store sensitive data like DB Credential, API Keys, Server Port and any other credentials.

---

### 📌 12. How to implement authentication in node js?
We can use JWT (JSON Web Token) for authentication and authorization both.


## How to implement JWT in node js application?

1. for generate jwt token : jwt.sign() 
2. For verify jwt token : jwt.verify()

Example: 
helper/jwt.js

```js
const jwt = require("jsonwebtoken");

exports.generateToken = (user) => {
  const payload = { id: user.id, email: user.email, role: user.role };
  return jwt.sign(payload, "secret_key", { expiresIn: "1d" });
};

```
After create jwt file, need to create a middleware JWT Middleware - middleware/authMiddleware.js

```js
const jwt = require("jsonwebtoken");

module.exports = (req, res, next) => {
  const token = req.headers["authorization"];

  if (!token) return res.status(401).json({ message: "No token provided" });

  try {
    req.user = jwt.verify(token, "secret_key");
    next();
  } catch (err) {
    res.status(403).json({ message: "Invalid or expired token" });
  }
};

```
Then we implemt the middleware in the route

```js
const verifyAuth  =   require("../middleware/authMiddleware.js")
const express = require("express");
const router = express.Router();

// public route
router.post("/login", authController.login);

// protected route 
router.get("/user", verifyAuth, authController.getUser);

module.exports = router;

```

---


## 📌 13. What is different between Authentication and Authorization?
 Authentication always comes before authorization. It verifies the user’s identity and ensures whether the user is valid or not.
 
 Authorization: The process of determining what actions a user is allowed to perform and what permissions they have. It occurs after authentication and is based on assigned roles and permissions.
  
---


## 📌 14.  What is Stateless Authentication?

Stateless authentication means the server does not store session data.
And server verifies the client’s identity using the token (JWT).

---

## 📌 15. How to Handle Logs?
We can use Winston or Morgan and it will handle all kinds of logs : error, warning, date time and custom log message.

---

## 📌 16. What is Buffer?
A Buffer in Node.js is a temporary memory space used to store binary data. It is commonly used when working with the file system, streams, or network operations.

### Why Buffers Are Needed 
JavaScript in browsers primarily works with strings and objects, but in Node.js you often handle:

🔸File system data (fs)

🔸Network data (HTTP, TCP)

🔸Image uploads

🔸Audio/Video streams

🔸Other raw binary data

These types of data are binary, not plain text.
Therefore, Node.js uses Buffers to efficiently store and process this binary data.

###  How Buffers Work Internally
Buffers are implemented in C++ behind the scenes for performance. It is used for work with file, images , video and network etc

### Important Buffer Methods

| Method                          | Meaning / Purpose                                                           |
| ------------------------------- | --------------------------------------------------------------------------- |
| **Buffer.from(data)**           | Creates a new Buffer from the given data (string, array, etc.).             |
| **Buffer.alloc(size)**          | Creates an empty Buffer of the specified size, filled with zeros.           |
| **Buffer.concat([buf1, buf2])** | Combines two or more Buffers into a single Buffer.                          |
| **buf.toString(encoding)**      | Converts the Buffer into a string (default: UTF-8).                         |
| **buf.write(string)**           | Writes a string into the Buffer.                                            |
| **buf.slice(start, end)**       | Returns a portion of the Buffer as a new sub-buffer (without copying data). |


---


## 📌 17. What is fs in Node.js?
The fs (File System) module in Node.js allows your application to read, write, update, delete, rename, and manage files or folders directly from the server.

### Common Use Cases

| Operation               | Description                  | Method            |
| ----------------------- | ---------------------------- | ----------------- |
| 📖 **Read a file**      | Load file content            | `fs.readFile()`   |
| ✍️ **Write to a file**  | Create or overwrite a file   | `fs.writeFile()`  |
| 🔁 **Append content**   | Add data to an existing file | `fs.appendFile()` |
| 🗑 **Delete a file**    | Remove a file                | `fs.unlink()`     |
| 📦 **Rename a file**    | Change file name             | `fs.rename()`     |
| 📁 **Create directory** | Make a new folder            | `fs.mkdir()`      |
| 🧹 **Remove directory** | Delete a folder              | `fs.rmdir()`      |

---

## 📌 17. What is a Child Process in Node.js?

In Node.js, a child process is a separate process that runs independently alongside your main Node.js application.
It allows you to execute system commands, run scripts, or perform heavy tasks outside the main event loop.

## 🧠 Why do we need Child Processes?
Node.js runs on a single thread, so heavy CPU tasks can block the event loop and slow your entire app.
Child processes help by:
✔ Running heavy tasks in the background
✔ Preventing blocking of the main thread
✔ Allowing parallel work
✔ Using multiple CPU cores
✔ Running external programs (Python, Shell, etc.)

## 🧩Methods to Create Child Processes

Node.js provides FOUR methods:

1️⃣ exec()
Runs a command in the shell, returns output as a string.

2️⃣ spawn()
Runs a command and streams output (best for long tasks).

3️⃣ fork()
Creates a new Node.js process and allows messaging between parent-child.

4️⃣ execFile()
Runs a file directly without a shell (faster + safer).

## 🧠 Child Process vs Cluster (Difference)

| Feature       | Child Process            | Cluster                        |
| ------------- | ------------------------ | ------------------------------ |
| Purpose       | Run commands/scripts     | Scale Node.js across CPU cores |
| Communication | Manual messaging         | Built-in load balancing        |
| Usage         | System tasks, heavy jobs | High traffic APIs              |

---



## 📌 18. What is Process?
process is a global object in Node.js that provides information and control over the current Node.js application. It exposes various runtime details such as:

Process ID

Platform (OS)

Node.js version

Environment variables

Execution arguments

Memory usage

Uptime


---


## 📌 19. What is Crypto?
It allows you to perform encryption, decryption, hashing, and digital signing operations to securely protect data.

---

## 📌 20. What are Streams?

Streams in Node.js are data-handling pipelines that allow you to read or write large amounts of data in small chunks, rather than loading everything into memory at once.

A simple example is watching a YouTube video while it's still loading — you receive the video in small parts instead of downloading the full file first.

🧠 Why Streams Are Needed

If you try to load a very large file (e.g., a 2 GB video) using fs.readFile(), Node.js will attempt to load the entire file into memory, which can:

Slow down the server

Increase memory usage

Even crash the server
With streams, Node.js reads the same file piece by piece, making it far more efficient and safe.

| Use Case                     | Description                                           |
| ---------------------------- | ----------------------------------------------------- |
| 📂 **Large File Handling**   | Read/write multi-GB files without crashing the server |
| 📦 **File Uploads**          | Handle user uploads in chunks                         |
| 🎥 **Video/Audio Streaming** | Stream media to users instead of sending full file    |
| 🧩 **Data Compression**      | Use `zlib` for compression/decompression              |
| 🔗 **Large API Responses**   | Send big responses without buffering everything       |


Types of Streams in Node.js

Readable Stream – reads data (e.g., fs.createReadStream)

Writable Stream – writes data (e.g., fs.createWriteStream)

Duplex Stream – can read and write (e.g., TCP sockets)

Transform Stream – modifies data while reading/writing (e.g., zlib compression)


### Example: Reading a File Using a Readable Stream

```js

const fs = require('fs');

const readStream = fs.createReadStream('bigfile.txt', 'utf8');

readStream.on('data', (chunk) => {
  console.log("Received chunk:", chunk);
});

readStream.on('end', () => {
  console.log("Finished reading file");
});


```
---

## 📌 21. What is Zlib?


---











