# Session 12: Terminal & npm

## Duration Breakdown
- **1 Hour**: Theoretical Explanation + Live Coding
- **1.5 Hours**: Practical Application (Student writes code)
- **0.5 Hours**: Review, Questions, Problem Solving

---

## Part 1: Theoretical Explanation + Live Coding (1 Hour)

### Section 1: What is Terminal?

**Definition:**
Terminal (or Command Line) is a text-based interface for interacting with your computer's operating system.

**Why Learn Terminal:**
- Faster than GUI for many tasks
- Essential for development
- Automation and scripting
- Server administration
- Version control (Git)
- Package management (npm)

**Common Terminals:**
- **Windows**: Command Prompt (cmd), PowerShell, Git Bash
- **macOS**: Terminal, iTerm2
- **Linux**: Terminal, GNOME Terminal

---

### Section 2: Basic Terminal Commands

## Navigation Commands

### pwd (Print Working Directory)
Shows your current location.

```bash
pwd
```

### ls (List)
List files and directories.

```bash
ls              # List files
ls -la          # List all files with details
ls -lh          # List with human-readable sizes
```

### cd (Change Directory)
Navigate between directories.

```bash
cd folder-name          # Go to folder
cd ..                   # Go up one level
cd /                    # Go to root
cd ~                    # Go to home directory
cd -                    # Go to previous directory
```

---

## File & Directory Commands

### mkdir (Make Directory)
Create a new directory.

```bash
mkdir folder-name
mkdir -p path/to/folder    # Create nested directories
```

### rmdir (Remove Directory)
Remove empty directory.

```bash
rmdir folder-name
```

### rm (Remove)
Delete files and directories.

```bash
rm filename.txt            # Remove file
rm -r folder-name          # Remove directory and contents
rm -rf folder-name         # Force remove (be careful!)
```

### touch
Create an empty file.

```bash
touch filename.txt
```

---

## File Operations

### cp (Copy)
Copy files and directories.

```bash
cp source.txt destination.txt          # Copy file
cp -r source-folder destination-folder  # Copy directory
```

### mv (Move)
Move or rename files.

```bash
mv old-name.txt new-name.txt          # Rename
mv file.txt folder/                    # Move to folder
```

### cat
Display file contents.

```bash
cat filename.txt
```

---

## Other Useful Commands

### clear
Clear the terminal screen.

```bash
clear
# Or
Ctrl + L
```

### echo
Print text to terminal.

```bash
echo "Hello World"
```

### history
Show command history.

```bash
history
```

### man
Show manual for a command.

```bash
man ls
```

---

### Section 3: Introduction to npm

**What is npm:**
npm (Node Package Manager) is the default package manager for Node.js.

**What is Node.js:**
Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine.

**npm Features:**
- Install packages
- Manage dependencies
- Run scripts
- Publish packages
- Version management

---

### Section 4: package.json

**What is package.json:**
A JSON file that contains metadata about your project and its dependencies.

**Essential Fields:**
```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "My awesome project",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.0"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  },
  "author": "Your Name",
  "license": "MIT"
}
```

---

### Section 5: Dependencies vs DevDependencies

## Dependencies
Packages needed for your application to run in production.

```bash
npm install express
```

**Examples:**
- Frameworks (Express, React)
- Libraries (Lodash, Axios)
- Utilities

---

## DevDependencies
Packages needed only during development.

```bash
npm install --save-dev jest
```

**Examples:**
- Testing frameworks (Jest, Mocha)
- Linters (ESLint, Prettier)
- Build tools (Webpack, Babel)
- Development servers

---

## peerDependencies
Dependencies that your package expects the consumer to provide.

---

## optionalDependencies
Dependencies that are optional but will be used if available.

---

### Section 6: npm Commands

## npm init
Initialize a new project.

```bash
npm init              # Interactive mode
npm init -y           # Skip questions (use defaults)
```

---

## npm install
Install packages.

```bash
# Install local package
npm install package-name

# Install specific version
npm install package-name@1.2.3

# Install as dev dependency
npm install --save-dev package-name
# Or
npm install -D package-name

# Install globally
npm install -g package-name

# Install all dependencies from package.json
npm install
```

---

## npm uninstall
Remove packages.

```bash
npm uninstall package-name
npm uninstall -g package-name
```

---

## npm update
Update packages.

```bash
npm update              # Update all
npm update package-name # Update specific
```

---

## npm list
List installed packages.

```bash
npm list               # Local packages
npm list -g             # Global packages
npm list --depth=0      # Top level only
```

---

## npm run
Run scripts defined in package.json.

```bash
npm run start
npm run test
npm run build
```

---

### Section 7: npx

**What is npx:**
npx is a package runner that executes npm packages without installing them globally.

**Benefits:**
- No need to install globally
- Always uses latest version
- Temporary execution

**Examples:**
```bash
# Run create-react-app without installing
npx create-react-app my-app

# Run prettier
npx prettier --write index.js

# Run typescript
npx tsc index.ts
```

---

### Section 8: npm Scripts

**Defining Scripts:**
```json
{
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "build": "webpack --mode production",
    "test": "jest",
    "lint": "eslint .",
    "format": "prettier --write ."
  }
}
```

**Running Scripts:**
```bash
npm run start
npm run dev
npm run build
```

**Pre and Post Scripts:**
```json
{
  "scripts": {
    "prestart": "echo 'Starting...'",
    "start": "node index.js",
    "poststart": "echo 'Started!'"
  }
}
```

---

### Section 9: package-lock.json

**What is package-lock.json:**
Automatically generated file that describes the exact tree of dependencies.

**Purpose:**
- Ensure consistent installs across machines
- Lock dependency versions
- Improve install speed
- Security auditing

**Should you commit it?**
Yes! Commit it to version control.

---

### Section 10: Semantic Versioning (SemVer)

**Version Format:**
```
MAJOR.MINOR.PATCH
```

**Examples:**
- `1.0.0` - First stable release
- `1.2.3` - Major 1, Minor 2, Patch 3

**Rules:**
- **MAJOR**: Incompatible API changes
- **MINOR**: Backwards-compatible functionality
- **PATCH**: Backwards-compatible bug fixes

**Version Ranges:**
```json
{
  "dependencies": {
    "package": "^1.2.3",    // >=1.2.3 <2.0.0
    "package": "~1.2.3",    // >=1.2.3 <1.3.0
    "package": "1.2.3",     // Exact version
    "package": "latest",    // Latest version
    "package": "*"         // Any version
  }
}
```

---

## Part 2: Practical Application (1.5 Hours)

### Exercise 1: Terminal Navigation (20 minutes)

**Task:**
Practice terminal navigation commands.

```bash
# Navigate to a directory
cd Documents

# List files
ls -la

# Create a new folder
mkdir my-project

# Go into the folder
cd my-project

# Go back
cd ..

# Remove the folder
rmdir my-project

# Create nested folders
mkdir -p project/src
mkdir -p project/public

# Navigate to project
cd project

# Create files
touch index.html
touch style.css
touch script.js

# List files
ls -la

# Go back to parent
cd ..

# Remove project folder
rm -rf project
```

---

### Exercise 2: Initialize npm Project (25 minutes)

**Task:**
Initialize a new npm project and install packages.

```bash
# Create project directory
mkdir my-app
cd my-app

# Initialize npm project
npm init -y

# View package.json
cat package.json

# Install dependencies
npm install express

# Install dev dependencies
npm install --save-dev nodemon

# Install multiple packages
npm install lodash axios

# View installed packages
npm list --depth=0

# View package.json
cat package.json
```

**Expected package.json:**
```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "dependencies": {
    "axios": "^1.6.0",
    "express": "^4.18.2",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "nodemon": "^3.0.1"
  }
}
```

---

### Exercise 3: Setup Project with Scripts (25 minutes)

**Task:**
Create a project with npm scripts.

```bash
# Create project
mkdir web-project
cd web-project

# Initialize npm
npm init -y

# Install development tools
npm install --save-dev live-server prettier

# Create index.html
echo '<!DOCTYPE html>
<html>
<head>
    <title>My Project</title>
</head>
<body>
    <h1>Hello World</h1>
</body>
</html>' > index.html

# Add scripts to package.json
# Edit package.json to add:
{
  "scripts": {
    "start": "live-server",
    "format": "prettier --write .",
    "test": "echo \"No tests yet\""
  }
}

# Run development server
npm run start

# Format code
npm run format
```

---

### Exercise 4: Using npx (20 minutes)

**Task:**
Practice using npx for various tools.

```bash
# Run create-react-app without installing
npx create-react-app my-react-app

# Run prettier without installing
npx prettier --check index.html

# Run typescript without installing
npx typescript --version

# Run http-server without installing
npx http-server -p 8080

# Clean up
rm -rf my-react-app
```

---

## Part 3: Review, Questions, Problem Solving (0.5 Hours)

### Common Terminal Mistakes (10 minutes)

**Mistake 1: Wrong Directory**
```bash
# Wrong
npm install

# Right - check current directory first
pwd
cd my-project
npm install
```

**Mistake 2: Not Using Tab Completion**
```bash
# Instead of typing full names
cd very-long-folder-name

# Use tab completion
cd very<TAB>
```

**Mistake 3: Forgetting -r in rm**
```bash
# Wrong - won't remove directory
rm folder

# Right
rm -r folder
```

**Mistake 4: Confusing cp and mv**
```bash
# cp copies (keeps original)
cp file.txt folder/

# mv moves (removes original)
mv file.txt folder/
```

---

### Common npm Mistakes (10 minutes)

**Mistake 1: Not Committing package-lock.json**
```bash
# Wrong - only commit package.json
git add package.json

# Right - commit both
git add package.json package-lock.json
```

**Mistake 2: Installing Globally When Local is Needed**
```bash
# Wrong
npm install -g express

# Right
npm install express
```

**Mistake 3: Not Understanding Version Ranges**
```bash
# ^1.2.3 means >=1.2.3 <2.0.0
# ~1.2.3 means >=1.2.3 <1.3.0
# 1.2.3 means exactly 1.2.3
```

**Mistake 4: Not Using npx for One-off Commands**
```bash
# Wrong - installing globally
npm install -g create-react-app
create-react-app my-app

# Right - using npx
npx create-react-app my-app
```

---

### Review Questions (10 minutes)

**Question 1:** What is the difference between `ls` and `ls -la`?
**Answer:** `ls` lists files, `ls -la` lists all files including hidden ones with details.

**Question 2:** What does `cd ..` do?
**Answer:** Moves up one directory level.

**Question 3:** What is the difference between dependencies and devDependencies?
**Answer:** Dependencies are needed in production, devDependencies only during development.

**Question 4:** What does `npm init -y` do?
**Answer:** Initializes a new npm project with default values without asking questions.

**Question 5:** What is npx used for?
**Answer:** To execute npm packages without installing them globally.

**Question 6:** What is package-lock.json?
**Answer:** A file that locks exact dependency versions for consistent installs.

**Question 7:** What does semantic versioning mean?
**Answer:** Versioning format MAJOR.MINOR.PATCH indicating API compatibility.

**Question 8:** How do you run a script defined in package.json?
**Answer:** Use `npm run script-name`.

---

### Practice Challenges (5 minutes)

**Challenge 1:** Create a directory structure using terminal commands only.

**Challenge 2:** Initialize an npm project and install 3 dependencies and 2 dev dependencies.

**Challenge 3:** Add start and dev scripts to package.json.

---

### Homework Assignment

**Task:** Set up a complete project using terminal and npm.

**Requirements:**
- Create project directory using terminal
- Initialize npm project
- Install live-server and prettier as dev dependencies
- Add scripts for start and format
- Create basic HTML file
- Run live-server using npm script
- Format code using prettier via npx

**Due Date:** Next session

---

## End of Session 12

**Next Session:** DevTools and Debugging (Elements/Console/Network/Sources, Breakpoints, console methods, Lighthouse)