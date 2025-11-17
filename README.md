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

For improve API performance we need to work over 2 things.
### 1. Code-Level Optimization
      🔸Use Async/Await, Promises for optimize scalable code 
      🔸Avoid heavy operations inside loops.
      🔸Use efficient array methods like (map, reduce)
      🔸Optimize Middleware : Unnecessary middleware slows Node.js app.
         Avoid too many app.use()
         Don’t use bodyParser for all routes
         Don’t log everything in production
         Use route-specific middleware
      🔸 Reduce JSON Response Size 
      🔸 We can use Load Testing (JMeter) and identify the slow API endpoint and optimize them
      🔸Optimize Database Queries : 
        Add proper indexes for DB table
        Avoid SELECT * from DB query
        Use caching for repeated queries 
        Use connection pooling for avoid too many connections error and fast response.
        

### 2. Infrastructure-Level Optimization
      🔸 Use Redis caching
      🔸 Use PM2 with Cluster
      🔸 Use CDN for static file like (images, video, pdf, document,fonts, css, js )
      🔸 Use load Balancer 
      🔸 Use connection pooling
      🔸 Optimize Environment



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

