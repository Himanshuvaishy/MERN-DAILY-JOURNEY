# ✅ 13. What is npm? What is the purpose of `package.json`?

---

### 🔹 1. What (Simple Definition)

- **npm** stands for **Node Package Manager**.
- It is the **default package manager for Node.js** that allows you to install, manage, and share third-party modules.

> `package.json` is a file that acts as the **blueprint** of your Node.js project.  
It holds metadata, dependencies, scripts, and configurations for your project.

---

### 🔹 2. Why (Purpose / Importance)

- **npm** makes it easy to:
  - Reuse open-source tools
  - Manage project dependencies
  - Install specific versions
- **`package.json`** helps:
  - Keep track of installed modules
  - Share your project with others
  - Automate tasks (via npm scripts)

---

### 🔹 3. How (Detailed Explanation)

#### ✅ What is npm?

- Comes with Node.js by default
- Used via terminal or shell

**Installing a package:**
```bash
npm install express

Installing a package globally:

bash
Copy
Edit
npm install -g nodemon

✅ What is package.json?
A JSON file generated using:

bash
Copy
Edit
npm init
Example:

json

{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My first app",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
🔹 4. Real-World Analogy
npm = App Store for JavaScript libraries.

package.json = A manifest or checklist of all the tools your project needs, like a grocery list for your code.

🔹 5. Interview-Ready Summary
npm helps install and manage third-party packages in Node.js projects.

package.json stores project info, scripts, and dependencies.

Both are essential for scalable, maintainable development.

💡 Tip
Use:

npm install to install packages

npm start to run scripts defined in package.json

npm outdated to check old packages

⚠️ Trap
Don’t delete package.json — without it:

You can’t track dependencies

Your app won’t be reproducible or shareable