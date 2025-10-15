---

### **Topic 1: Introduction to GitHub Actions**

#### ✅ What is GitHub Actions?
GitHub Actions is a **CI/CD (Continuous Integration and Continuous Deployment)** platform built into GitHub. It allows you to **automate workflows** directly in your repository, such as:
- Running tests when you push code
- Building and deploying applications
- Automating repetitive tasks (e.g., labeling issues, sending notifications)
#### ✅ Why use GitHub Actions?
- **Integrated with GitHub**: No extra setup needed.
- **Cross-platform**: Works on Linux, Windows, macOS.
- **Customizable**: Use pre-built actions or create your own.
- **Scalable**: Supports matrix builds and parallel jobs.

#### ✅ Key Concepts
- **Workflow**: A YAML file that defines automation (located in `.github/workflows/`).
- **Job**: A set of steps that run on the same runner.
- **Step**: A single task (e.g., run a command or use an action).
- **Action**: A reusable piece of code (from GitHub Marketplace or custom).
- **Runner**: The machine that executes your jobs (GitHub-hosted or self-hosted).

---

#### ✅ Example: Hello World Workflow
Create a file: `.github/workflows/hello.yml`

```yaml
name: Hello World Workflow

on: push

jobs:
  say-hello:
    runs-on: ubuntu-latest
    steps:
      - name: Print Hello
        run: echo "Hello, GitHub Actions!"
```

**What happens?**
- Trigger: `on: push` → Runs when you push code.
- Job: `say-hello` runs on `ubuntu-latest`.
- Step: Prints a message.

---

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f4ca1018-d20f-41ee-bfef-cf0b8c03a6c9" />

---

### ✅ Moving to **Topic 2: Basic Workflow Structure**

#### **Anatomy of a Workflow File**
A workflow is defined in a **YAML file** inside `.github/workflows/`. Here’s the structure:

```yaml
name: <Workflow Name>

on: <Event Trigger>

jobs:
  <job_id>:
    runs-on: <Runner>
    steps:
      - name: <Step Name>
        run: <Command>
```

#### **Example: Simple Workflow**
```yaml
name: CI Workflow

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

**Explanation:**
- `name`: Workflow name (appears in GitHub Actions tab).
- `on`: Trigger (here, `push` to `main` branch).
- `jobs`: Defines tasks (here, `build` job).
- `steps`: Each step runs a command or uses an action.

---

### ✅ Topic 3: **Triggers (Events)**

Triggers define **when** your workflow runs. They are specified using the `on:` keyword in your workflow file.

---

#### **Common Triggers**
1. **push**  
   Runs when code is pushed to the repository.
   ```yaml
   on: push
   ```
   Or specify branches:
   ```yaml
   on:
     push:
       branches:
         - main
         - 'feature/*'
   ```

2. **pull_request**  
   Runs when a pull request is opened, synchronized, or closed.
   ```yaml
   on: pull_request
   ```

3. **workflow_dispatch**  
   Manual trigger from the GitHub UI.
   ```yaml
   on: workflow_dispatch
   ```

4. **schedule**  
   Runs on a cron schedule.
   ```yaml
   on:
     schedule:
       - cron: '0 0 * * *' # Every day at midnight
   ```

---

#### ✅ Example: Multiple Triggers
```yaml
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
  workflow_dispatch:
```

---

#### ✅ Best Practices
- Use **branch filters** to avoid unnecessary runs.
- Combine triggers for flexibility (e.g., `push` + `pull_request`).
- For scheduled workflows, ensure they are **efficient** (avoid heavy jobs at frequent intervals).

---

### ✅ Topic 4: **Jobs and Steps**

Jobs and steps are the **building blocks** of a workflow. Let’s break them down:

---

#### **What is a Job?**
- A **job** is a collection of steps that run on the **same runner**.
- Jobs can run **in parallel** or **sequentially** (using `needs:`).

Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Run tests
        run: npm test
```
Here:
- `build` runs first.
- `test` runs **after** `build` because of `needs: build`.

---

#### **What is a Step?**
- A **step** is an individual task inside a job.
- Steps can:
  - Run shell commands (`run:`)
  - Use actions (`uses:`)

Example:
```yaml
steps:
  - name: Print message
    run: echo "Hello from a step!"
```

---

#### ✅ Common Keywords
- `runs-on`: Defines the OS (e.g., `ubuntu-latest`, `windows-latest`, `macos-latest`).
- `needs`: Makes jobs dependent on others.
- `strategy`: For matrix builds (we’ll cover later).

---

#### ✅ Best Practices
- Keep jobs **modular** (e.g., separate build and test).
- Use `needs:` for **clear dependencies**.
- Use **descriptive names** for steps.

---

### ✅ Topic 5: **Using Actions**

Actions are **reusable components** that perform specific tasks in your workflow. They save time and reduce boilerplate code.

---

#### **Types of Actions**
1. **Official GitHub Actions**  
   Example: `actions/checkout` (checks out your repo).
2. **Community Actions**  
   Available in GitHub Marketplace.
3. **Custom Actions**  
   You can create your own (JavaScript or Docker-based).

---

#### ✅ How to Use an Action
Use the `uses:` keyword in a step:
```yaml
steps:
  - name: Checkout code
    uses: actions/checkout@v3

  - name: Setup Node.js
    uses: actions/setup-node@v3
    with:
      node-version: '18'
```

**Explanation:**
- `uses:` specifies the action and version.
- `with:` passes input parameters to the action.

---

#### ✅ Example: Using Multiple Actions
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: npm test
```

---

#### ✅ Best Practices
- Always **pin versions** (e.g., `@v3`) for stability.
- Check **Marketplace ratings** before using community actions.
- Avoid unnecessary actions to keep workflows fast.

---

**GitHub Actions** are similar to **Terraform public modules** in these ways:  
- Both are **reusable components** created by the community or official sources.
- They help you **avoid reinventing the wheel** by providing ready-made solutions.
- You can **customize them with inputs** (`with:` in Actions, variables in Terraform).
- They are versioned (e.g., `actions/checkout@v3` vs. `terraform module version`).

**Difference:**  
- Terraform modules manage **infrastructure** (servers, networks, etc.).
- GitHub Actions automate **CI/CD workflows** (build, test, deploy, etc.).

---

✅ So think of Actions as **workflow plugins**, just like Terraform modules are **infrastructure plugins**.

---
