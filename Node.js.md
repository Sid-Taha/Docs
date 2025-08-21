# **Node.js Learning Guide**

## **0. What is HTML and CSS?**

* **HTML (HyperText Markup Language)**: It is the standard language used to create the structure of web pages. HTML defines the content and layout of a page using various tags, such as `<h1>`, `<p>`, `<div>`, etc.
* **CSS (Cascading Style Sheets)**: It is used to style and layout web pages. CSS controls the color, font, spacing, layout, and overall appearance of HTML elements.

## **1. What is JavaScript and Mobile Check and its Frameworks, Libraries?**

* **JavaScript (JS)**: A dynamic programming language used for creating interactive effects on web pages. It can also be used in mobile app development (via frameworks like React Native).
* **Mobile JS frameworks**:

  * **React Native**: A framework for building native mobile apps using JavaScript.
  * **Ionic**: A framework for building cross-platform mobile apps using web technologies.
  * **Vue.js**: Can also be used in mobile apps with frameworks like NativeScript.

## **2. Node.js: Framework or Library?**

* **Node.js** is a **runtime environment** built on Chrome’s V8 JavaScript engine. It is NOT a framework or a library. It allows JavaScript to be run server-side.

## **3. Browser JS Engines:**

* **V8 (Chrome)**: Google Chrome's JavaScript engine that compiles and executes JS.
* **SpiderMonkey (Mozilla Firefox)**: Firefox's JS engine for compiling and executing JS.
* **Nitro (Safari)**: Safari’s JS engine optimized for performance.

## **4. Can You Run JavaScript Outside the Browser?**

* Yes! JavaScript can be run outside the browser using **Node.js**, allowing you to build server-side applications.

## **5. V8 + C++ = Node.js by Ryan Dahl**

* Node.js was created by Ryan Dahl using the **V8 engine** (which compiles JavaScript to machine code) and **C++** to create a runtime environment that allows JavaScript to run outside the browser.

## **6. Node.js: Framework or Library?**

* Node.js is a **runtime environment**, not a framework or library. It provides an environment to execute JavaScript code outside the browser.

## **7. You Can Run Node.js Outside the Browser**

* Node.js allows you to run JavaScript code in various environments like your computer's command line, creating server-side applications, APIs, and more.

## **8. Download Node.js LTS Version**

* Go to [Node.js official website](https://nodejs.org/) and download the **LTS** (Long-Term Support) version for stability and compatibility with most tools.

## **9. Check Node in CMD by Writing 'node'**

* Open the command line (CMD) and type:

  ```bash
  node
  ```

  This will open the Node.js REPL (Read-Eval-Print-Loop) where you can run JavaScript directly.

## **10. Hello World in VSCode (Node.js)**

* Create a new file `main.js` and write the following code:

  ```javascript
  console.log('Hello, World!');
  ```

  Run the file using:

  ```bash
  node main.js
  ```

## **11. Node --help**

* To get a list of Node.js command line options, use:

  ```bash
  node --help
  ```

  This shows all the available commands and options for Node.js.

## **12. Node --watch main.js**

* Use the `--watch` flag to automatically restart your application when any changes are made to your code:

  ```bash
  node --watch main.js
  ```

## **13. What is npm?**

* **npm (Node Package Manager)** is the default package manager for Node.js. It is used to install and manage packages (libraries and tools) in your Node.js projects.

## **14. Alert() Will Not Work in Node, WebAPI Will Run This Alert in Browser**

* `alert()` is part of the browser's Web API and cannot be used in Node.js. You would need to use alternatives like `console.log()` in Node.

## **15. Fetch, Alert, Window - Not Available in Node.js**

* `fetch()`, `alert()`, and `window` are part of the browser environment. These are not available in Node.js. However, you can use libraries like **node-fetch** for HTTP requests.

## **16. Check Window Object in Browser**

* The `window` object is available in browsers and holds information like the document, location, and other browser-specific APIs. In Node.js, this object does not exist.

## **17. Two Different Environments to Run JS (Browser vs Node.js)**

* **Browser**: JavaScript runs inside a browser, with access to the DOM, `window`, `alert()`, and other Web APIs.
* **Node.js**: JavaScript runs on a server, with access to file system, networking, and operating system APIs, but without browser-specific features like `window` or `alert()`.

## **18. setTimeout is Part of Browser API, Node.js Devs Re-Implemented These Parts for Node.js**

* In the browser, `setTimeout()` is part of the Web API. Node.js re-implements this functionality in its own environment using **timers module**.

## **19. Three Types of Modules in Node.js**

* **Built-in Modules**: Modules that come with Node.js, e.g., `fs`, `path`, `http`.
* **Third-party Modules**: Modules installed using npm, e.g., `express`, `lodash`.
* **Custom Modules**: Modules that you write yourself, using `module.exports`.

## **20. Example of Using fs Module in Node.js**

```javascript
const fs = require('fs');

// Read file synchronously
const content = fs.readFileSync('note.txt', 'utf-8');
console.log(content);
```

## **21. Check fs in Node.js Docs**

* The official Node.js documentation provides extensive information about the **fs module**. You can access it [here](https://nodejs.org/dist/latest-v16.x/docs/api/fs.html).

## **22. Your Code is Running in a Wrapper Function**

* Every Node.js file is wrapped in a function to avoid polluting the global namespace. It’s similar to this:

  ```javascript
  (function(exports, require, module, __filename, __dirname) {
      // Your code here
  })();
  ```

## **23. Require Checks 3rd-party Module, Then Built-in Module, Then Throws Error**

* When using `require()`:

  * First, it checks for a 3rd-party module.
  * If not found, it checks for built-in modules.
  * If not found, it throws an error.

## **24. Require Custom Modules**

* To require custom modules, use the relative path, like:

  ```javascript
  const myModule = require('./myModule.js');
  ```

## **25. Understanding `require` and `fs`**

* `require()` is used to load modules, either built-in, third-party, or custom.
* `fs` is the file system module in Node.js, used to interact with the file system, e.g., reading or writing files.

---

This guide will help you get started and deepen your understanding of Node.js, from basic concepts to working with modules. You can expand on each of these sections by diving deeper into documentation and practice.
