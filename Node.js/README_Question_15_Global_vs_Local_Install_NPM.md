# ✅ 15. How do you install a package globally and locally using npm?

---

### 🔹 1. What (Simple Definition)

- **Local installation**: Installs the package in the current project directory under `node_modules`.
- **Global installation**: Installs the package system-wide so it can be used from anywhere via the command line.

---

### 🔹 2. Why (Purpose / Importance)

- Use **local** install when the package is only needed for that specific project.
- Use **global** install when the package is a **CLI tool** (e.g., nodemon, typescript) that you want to use in multiple projects or directly from the terminal.

---

### 🔹 3. How (With Commands)

#### ✅ Local Installation

```bash
npm install <package-name>
Installed in the project's node_modules/ folder.

Automatically added to dependencies in package.json if it exists.

Example:

bash
Copy
Edit
npm install express
✅ Global Installation
bash
Copy
Edit
npm install -g <package-name>
Installed system-wide.

Available in terminal/command prompt anywhere.

Example:

bash
Copy
Edit
npm install -g nodemon
✅ Where Are They Installed?
Type	Location	Usage Scope
Local	./node_modules/	Only in current project
Global	Depends on system (e.g., /usr/lib)	Anywhere on your machine

🔹 4. Real-World Analogy
Local install = Tools in your own toolbox (used at home).

Global install = Tools in your vehicle (used wherever you go).

🔹 5. Interview-Ready Summary
Use npm install for local packages needed in a project.

Use npm install -g for CLI tools used globally.

Understand when to keep packages project-specific vs system-wide.

💡 Tip
Check where a global package is installed:

bash
Copy
Edit
npm list -g --depth=0
⚠️ Trap
Avoid installing application-specific packages globally — it can create version conflicts and portability issues when sharing the project.