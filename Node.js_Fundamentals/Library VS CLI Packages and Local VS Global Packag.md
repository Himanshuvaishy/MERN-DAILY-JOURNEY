<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Library VS CLI Packages and Local VS Global Packages

Here are simple notes explaining Library vs CLI Packages and Local vs Global Packages in Node.js context:

***

# Library vs CLI Packages and Local vs Global Packages in Node.js

## Library vs CLI Packages

- **Library Packages:**
    - Provide reusable code, functions, or utilities that developers use inside their applications.
    - Typically imported or required in the code to access functionalities.
    - Example: `lodash` is a utility library with functions for manipulating arrays, objects, etc.
    - Libraries do not expose commands on the command line.
- **CLI (Command Line Interface) Packages:**
    - Designed to be run as executable commands from the terminal.
    - Provide tools or utilities accessed via command line.
    - Often installed globally so that commands are available system-wide.
    - Example: `eslint` is a CLI tool for linting JavaScript code, used via the terminal.
    - CLI packages often have a shebang line (`#!/usr/bin/env node`) which allows them to be run as native commands.


## Local vs Global Packages

- **Local Packages:**
    - Installed within a project's directory (inside `node_modules`).
    - Only available for that specific project.
    - Installed using `npm install package-name` or `yarn add package-name`.
    - Usage example: when you want to include a library only in one project.
- **Global Packages:**
    - Installed globally on the system.
    - Available from anywhere in the command line.
    - Installed using `npm install -g package-name`.
    - Generally used for CLI tools that you want to use across multiple projects or directly from the terminal.


## Summary Table

| Aspect | Library Package | CLI Package | Local Package | Global Package |
| :-- | :-- | :-- | :-- | :-- |
| Purpose | Code reuse inside apps | Command line tools | Project-specific package | System-wide CLI commands |
| Usage | Imported via `require` or `import` | Run as executable commands | Installed in project folder | Installed system-wide |
| Installation | `npm install package-name` | `npm install -g package-name` | `npm install package-name` | `npm install -g package-name` |
| Accessibility | Only within project | Any terminal session | Only within project | From any terminal location |
| Example | `lodash`, `axios` | `eslint`, `nodemon` | `express` in one app | `http-server`, `npm` |


***

These notes provide a clear distinction between libraries and CLI packages, and between local and global package installation, helping in understanding their usage and management in Node.js development.
<span style="display:none">[^1][^2][^3][^4][^5][^6][^7][^8]</span>

<div style="text-align: center">⁂</div>

[^1]: https://stackoverflow.com/questions/61399496/difference-between-library-package-module-in-js

[^2]: https://www.reddit.com/r/learnjavascript/comments/17qxzpk/modules_packages_libraries_etc/

[^3]: https://nodejs.org/api/cli.html

[^4]: http://nodesource.com/blog/nodejs-package-manager-comparative-guide-2024/

[^5]: https://betterprogramming.pub/handling-class-libraries-in-node-js-with-and-without-typescript-39b73b2186b6

[^6]: https://www.codersrevolution.com/blog/why-writing-cli-scripts-in-commandbox-is-better-than-node

[^7]: https://www.milesweb.com/blog/technology-hub/what-is-npm/

[^8]: https://www.w3schools.com/nodejs/nodejs_command_line.asp

