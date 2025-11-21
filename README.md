## JIT (Just in time) Compiller

<b>First we need to know what is compiller and interpreter</b>
- Interpreter: execute code line by line. It is slow because of line by line execution.

- Compiller: execute hole code at a time. It is fast but debugging is tough.

Here comes in mind if there is somthing that provides the benifit of Interpreter and compiller. This Introduce us the concept *`JIT Compiller.`*

জাভাস্ক্রিপ্ট কোড ইঞ্জিনের ভেতর প্রথমে একটি ইন্টারপ্রেটারের ভেতর দিয়ে যায়। ইন্টারপ্রেটার যখন লাইন ধরে ধরে কোড এক্সিকিউট করতে থাকে, প্রোফাইলার তখন কোন স্টেটমেন্ট কতবার করে রান হচ্ছে এটা হিসাব করে রাখে। কোডের একই অংশ যদি একাধিকবার এক্সিকিউট হয় প্রোফাইলার তখন এটিকে ‘ওয়ার্ম (Warm)’ হিসেবে শনাক্ত করে। এই সংখ্যাটি আরো বাড়তে থাকলে এক সময় এটিকে ‘হট কোড’ বলা হয়।

**বেইজলাইন কম্পাইলারঃ**
এখানে কোডের ওয়ার্ম সেকশনগুলোকে বাইটকোডে রুপান্তর করা হয়। এই বাইটকোড পরবর্তীতে একটি ইন্টারপ্রেটার দিয়ে রান করানো হয় যেটা এই ধরনের বাইটকোডের জন্য অপটিমাইজড। 

**অপটিমাইজিং কম্পাইলারঃ** 
মনিটর বা প্রোফাইলার দ্বারা শনাক্ত হওয়া হট পার্টগুলো এই কম্পাইলারের কাছে পাঠানো হয়। এর মূল কাজ হলো হট পার্টগুলোর আরেকটি অপটিমাইজড ভার্সনে সংরক্ষণ করে রাখা যেটি আরো দ্রুত রান করবে। এই কাজটি করার জন্য জাভাস্ক্রিপ্ট ইঞ্জিনে ‘শেইপ’ ধারনাটি ব্যবহৃত হয়। অনেক বেশিবার রান হওয়া কোড যদি সরাসরি মেশিন কোডে রূপান্তর করে সেটাকে বার বার ব্যবহার করা যায় তাহলে প্রোগ্রাম অনেক ভালো পারফর্ম করবে। অপটিমাইজিং কম্পাইলার আমাদের জন্য এই কাজটিই করে দেয়। 


## JS Engine vs JS Runtime

### 1. JavaScript Engine

- It is the component that parses, interprets, and executes JavaScript code.

- Examples: V8 (Chrome, Node.js), SpiderMonkey (Firefox), JavaScriptCore (Safari).
> Think of it as the “brain” that understands and runs JS code

### 2. JavaScript Runtime
- A runtime is an environment that provides JS code the ability to interact with the outside world (not just execute it).

- JS engine (executes JS)

- APIs / built-in objects (like console, setTimeout, fetch, DOM in browsers)

- Event loop & callback queue (for asynchronous code)

Examples:

- Browser Runtime: V8 + DOM API + Web APIs + Event Loop

- Node.js Runtime: V8 + Node APIs (fs, http) + Event Loop

> Think of runtime as the “whole environment” that lets JS actually do things beyond calculations.

## Interview-friendly answer

- “The JavaScript engine is part of the runtime; it executes JS code using parsing and JIT compilation. A JS runtime, like a browser or Node.js, includes the engine plus additional APIs and an event loop that allow JavaScript to interact with the environment and handle asynchronous operations.”


## Node JS things

- Node js not have window object . Node has own global object. Thats kindda browser window.
Example
```js
console.log(global);
```

When we write var it directly save in window in our browser. But for node it isn't save as a global object.

**Example:**
In browser: 
```js
   var a = 5;
   console.log(window.a); // 5
```
In Node: 
```js
   var a = 5;
   console.log(global.a); // undefined
```

## Node module
- Every .js file is a module.
- It ensure every js file is individually encapsulated like one file can't override another. When we use var it is an global object in browser and that is override when we declare same variable in two diffrent js file. Node reduces this issues.

**import module:**

Node internally use IIFEs  
      
`Node Basic/people.js`
```js
var people = ['sakib', 'tamim', 'mash'];
var a = 6;
function test() {
    console.log("test");
}
console.log(module);
module.exports = people; 
```

`Node Basic/index.js`
```js
const people =  require('./people');
console.log(people);
```

## fs module
Use to write a file using fs module

`Synchronous Way:`
```js
const fs = require('fs');

fs.writeFileSync('demo.js', 'Hello, JS');
```
`Asynchronous way: `
```js
fs.writeFile('demo.js', 'Hello JS', (err) => {
    console.log("Done!");
});

// or
await fs.promises.writeFile('demo.js', 'Hello JS');
```

**Read a file Using fs**

```js
const fs = require('fs');

fs.writeFileSync('demo.js', 'Hello, JS');

fs.readFile('demo.js', (err, data) => {
    console.log(data.toString()); // Hello, JS
})
console.log('Hello')
```
Ouput: 
```
   Hello
   Hello, JS
```
## Important module you need to learn
1. os module
2. fs module
3. event module
4. http module
## Why we use Asynchronous instead of sync

**Simple Analogy Synchronous:**

- You go to a coffee shop → order coffee → and stand there until coffee is made.

Everyone behind you waits.

**Asynchronous:**

You place your order → sit down → they notify you when it's ready.

Other people keep ordering.

`Node.js is the asynchronous coffee shop model.`

## How Browser and Node does Asynchronous task

Both **Node.js** and **Browser JavaScript** use the asynchronous coffee shop model —
because **JavaScript itself is single-threaded,** and both environments use:

- Event Loop
- Callback queue
- Non-blocking async APIs

**BUT…**
HOW they do it is different because the environments are different.

*However, JavaScript alone cannot do async tasks like timers, HTTP requests, file I/O.It needs a runtime.*

- browser and Node.js provide asynchronous non-blocking behavior using the event loop. Browsers use Web APIs for async tasks, while Node.js uses libuv and non-blocking I/O. So both follow the “asynchronous coffee shop” model — but the underlying implementation is different.


## Node JS Stream and Buffer
**Buffer:** A Buffer is a temporary memory where Node.js stores binary data before processing it.

**Stream:** A Stream is a continuous flow of data, delivered in chunks instead of all at once.

`Example:`

> Think about YouTube vidieo. The buffer just needs some chunks (not full) to start the video.
Stream keeps sending chunks while you watch.
Player keeps consuming chunks from the buffer.


## How does a port run on your localhost?

When you write something like:
```js
app.listen(5000);
```
You are telling your computer:

**“Reserve port 5000 and keep listening for incoming requests.”**

Your Node.js program becomes the owner of that port
and the OS (Windows/Mac/Linux) handles the rest.

What actually happens (simple explanation)
- 1. Your computer has an Operating System (OS)

The OS controls all ports from 0 – 65535.

- 2. When Node.js calls listen(port)

Node asks the OS:

“Can I use port 5000? I want to listen for incoming HTTP requests.”

- 3. OS checks

If the port is free → OS marks it as occupied by your Node.js process.

- 4. Your server now waits on that port

Whenever you type in browser:

```js
http://localhost:5000
```
The browser sends a request to your OS:

> “Send this request to the application listening on port 5000.”

The OS forwards the request → Node.js receives it → sends a response back.

**Summary**

- Node.js asks OS to open a port
- OS reserves the port for Node
- Your browser sends request to that port
- Node receives the request and responds


## 2. How does Postman test APIs?

Postman is simply an HTTP client.
Same as a browser, but more powerful.

When you type:
```js
GET http://localhost:5000/users
```
**Step 1:** Postman sends the request to OS

Postman → OS → “Send this to localhost on port 5000”

**Step 2:** OS routes request to port 5000

Your Node.js server is listening:

```js
app.listen(5000);
```

The OS delivers Postman’s request to your Node app.

**Step 3:** Node.js processes the request

Node reads the route:
```js
app.get('/users', (req, res) => {
   res.json([...]);
});
```
**Step 4:** Node sends response back to Postman

OS → Postman → displays result


## MVC Architectural Design:

**How MVC Works (Step-by-Step Flow):**

MVC (Model–View–Controller) is a software design pattern used to separate an application into three independent components:

`Model <--> Controller <--> View`

**Responsibilities of Model:**

- Store data – like users, products, posts

- Retrieve data – fetch from database

- Update data – modify or delete entries

- The Model does not care about the UI. It only handles data and logic

The Controller is the middleman between Model and View.
- View handles the UI / design part writen in HTML, CSS

**MVC PROCESS:**
1. User sends a request(Example: /users/login)

2. Controller receives the request. Controller decides what needs to be done.

3. Controller calls the Model. Model checks database, applies business logic.

4. Model returns the data Success or error.

5. Controller sends data to the View

View displays the output (HTML, JSON, UI).

![alt text](image.png)


## 1 Tire, 2 Tire, 3 Tire and multilevel Architecture:

**1 Tire:**
> Everything runs in a single system / single machine.
There is no separation between layers.
No network, no external server, no client-server communication.Everything happens locally.

**Example of One-Tier Architecture:**

- Desktop apps
- Local DB apps
- Simple games
- Calculator apps / ms word
- Student/college projects
- Offline software

**2-Tier Architecture**

**Definition:**

2-tier architecture is a type of software design where there are only two parts:

> There is no middle layer to handle logic or security.

- Client (front-end) – The part you use, like a program or app.

- Server (back-end) – The part that stores and sends data, like a database.

The client `talks directly` to the server.

Example:

- *client app talks straight to MySQL*
- *MS Access + SQL Server*
## 3-Tier Architecture
3-tier architecture is a software design pattern where the application is divided into three layers:
- Presentation Layer (Client - UI)
- Application Layer (Business Logic / Middle Tier)
- Data Layer (Database / Storage)

## Why 3-Tier Architecture is Needed
####  1. Problem with 2-Tier Architecture

- Client talks directly to database.

- For small apps, this works fine.

- But when your app grows bigger:

**Problems:**

*Scalability Issue:*

Every client connects directly to the database. Database can get overloaded if many clients connect at once.

*Security Issue:*

Direct access to database from clients → sensitive data exposed.

*Maintenance Issue:*

If you want to change business rules, you must update every client.

*Flexibility Issue:*

Hard to support different client types (web, mobile, desktop) without duplicating logic.


