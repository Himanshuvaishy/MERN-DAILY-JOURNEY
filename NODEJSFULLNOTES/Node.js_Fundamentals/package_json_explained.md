# 📦 Understanding `package.json` in Depth

The **`package.json`** file is the **blueprint (manifest)** of a Node.js project.  
It contains metadata about your project, scripts, and all dependencies required to run/build it.  

Think of it as the **ID card + instruction manual** for your project.  

---

## 🔹 Basic Example of `package.json`
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "A sample Node.js project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  },
  "keywords": ["nodejs", "project", "example"],
  "author": "Himanshu",
  "license": "MIT",
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^6.9.0"
  },
  "devDependencies": {
    "nodemon": "^3.0.0",
    "jest": "^29.0.0"
  }
}
```

---

## 🔹 Key-Value Pairs Explained

### 1. `"name"`
- The name of your project/package.
- Must be **lowercase**, no spaces.
- Example: `"my-app"`

### 2. `"version"`
- Current version of your project.
- Follows **Semantic Versioning** (`MAJOR.MINOR.PATCH`)
  - `1.0.0` → Major version 1, no new features (0 minor), no fixes (0 patch).

### 3. `"description"`
- A short explanation of your project.
- Helpful for **npm registry** or collaborators.

### 4. `"main"`
- Entry point of your app.
- Node.js loads this file when someone imports your package.
- Example: `"main": "index.js"`

### 5. `"scripts"`
- Custom commands you can run with `npm run <script>`.
- Example:
  ```json
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "jest"
  }
  ```
- Run with:
  ```bash
  npm start     # runs "node index.js"
  npm run dev   # runs "nodemon index.js"
  npm test      # runs "jest"
  ```

### 6. `"keywords"`
- Array of strings.
- Used for discoverability on npm registry.
- Example: `"keywords": ["nodejs", "project"]`

### 7. `"author"`
- Who created the project.
- Example: `"author": "Himanshu"`

### 8. `"license"`
- Specifies license type (like MIT, ISC, Apache).
- Important for open-source projects.

### 9. `"dependencies"`
- Libraries/packages your project needs **to run in production**.
- Example:
  ```json
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^6.9.0"
  }
  ```
- Installed via:
  ```bash
  npm install express
  ```

### 10. `"devDependencies"`
- Packages needed **only for development/testing**.
- Not required in production.
- Example:
  ```json
  "devDependencies": {
    "nodemon": "^3.0.0",
    "jest": "^29.0.0"
  }
  ```
- Installed via:
  ```bash
  npm install nodemon --save-dev
  ```

---

## 🔹 Dependencies vs DevDependencies

| Feature | dependencies | devDependencies |
|---------|--------------|-----------------|
| Purpose | Needed for running the project in production | Needed only for development or testing |
| Examples | express, mongoose, react | nodemon, jest, eslint |
| Installed in production? | ✅ Yes | ❌ No |
| Install command | `npm install package` | `npm install package --save-dev` |

---

## 🔹 Other Less Common Keys

- `"repository"` → GitHub/GitLab repo link  
- `"bugs"` → URL/email for reporting issues  
- `"homepage"` → Website of the project  
- `"engines"` → Defines which Node.js version is supported  
  ```json
  "engines": {
    "node": ">=16"
  }
  ```

---

## ✅ Summary
- `package.json` = project’s **manifest file**.  
- Contains **metadata, scripts, dependencies, and configs**.  
- `"dependencies"` → for production.  
- `"devDependencies"` → for development only.  
