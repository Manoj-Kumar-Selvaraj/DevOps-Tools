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

### ✅ Topic 6: **Environment Variables & Secrets**

Environment variables and secrets are essential for passing configuration and sensitive data into your workflows.

---

#### **Environment Variables**
- Use `env:` to define variables at **workflow**, **job**, or **step** level.

Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      NODE_ENV: production
    steps:
      - name: Print environment variable
        run: echo "Environment is $NODE_ENV"
```

**Scope:**
- Workflow level → available to all jobs.
- Job level → available to steps in that job.
- Step level → available only in that step.

---

#### **Secrets**
- Secrets are **encrypted values** stored in your repo settings.
- Common use: API keys, tokens, passwords.

**How to add secrets:**
1. Go to **Repo → Settings → Secrets and variables → Actions**.
2. Add a new secret (e.g., `MY_SECRET`).

**Usage in workflow:**
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Use secret
        run: echo "Secret is ${{ secrets.MY_SECRET }}"
```

---

#### ✅ Best Practices
- Never hardcode sensitive data in workflows.
- Use **GitHub Secrets** for credentials.
- Combine secrets with environment variables for flexibility.

---

### ✅ Topic 7: **Matrix Builds**

Matrix builds allow you to **run the same job across multiple configurations**—like different OSes, languages, or versions—**in parallel**. This is super useful for testing compatibility.

---

#### **Why Use Matrix Builds?**
- Test across multiple environments without writing separate jobs.
- Saves time because jobs run **in parallel**.
- Great for libraries or apps that support multiple versions.

---

#### ✅ Example: Node.js Matrix
```yaml
name: Node.js CI

on: push

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [14, 16, 18]
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
```

**What happens?**
- GitHub creates **3 parallel jobs**:
  - One with Node.js 14
  - One with Node.js 16
  - One with Node.js 18

---

#### ✅ Advanced Matrix Example
You can combine multiple dimensions:
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node-version: [16, 18]
```
This creates **4 jobs**:
- Ubuntu + Node 16
- Ubuntu + Node 18
- Windows + Node 16
- Windows + Node 18

---

#### ✅ Best Practices
- Use matrix for **testing**, not for heavy deployments.
- Limit combinations to avoid excessive runs.
- Use `fail-fast: false` if you want all jobs to complete even if one fails.

---
### ✅ Topic 8: **Caching & Artifacts**

Caching and artifacts help **speed up workflows** and **share data between jobs**.

---

#### **Caching**
- Caching stores dependencies or build outputs to **avoid re-downloading** in future runs.
- Common use: npm, pip, Maven dependencies.

Example:
```yaml
steps:
  - uses: actions/checkout@v3
  - uses: actions/setup-node@v3
    with:
      node-version: '18'
  - uses: actions/cache@v3
    with:
      path: ~/.npm
      key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-node-
  - run: npm install
```

**Explanation:**
- `path`: Directory to cache.
- `key`: Unique key for cache (based on OS and lock file).
- `restore-keys`: Fallback keys if exact match not found.

---

#### **Artifacts**
- Artifacts allow you to **upload files** from one job and **download them in another**.
- Useful for sharing build outputs, logs, or reports.

Example:
```yaml
# Upload artifact
- name: Upload build
 /upload-artifact@v3
  with:
    name: build-output
    path: dist/

# Download artifact
- name: Download build
  uses: actions/download-artifact@v3
  with:
    name: build-output
```

---

#### ✅ Best Practices
- Use caching for **dependencies**, not for large binaries.
- Use artifacts for **job-to-job communication**, not permanent storage.
- Clean up unnecessary files before uploading artifacts.

---
### ✅ Topic 9: **Custom Actions**

Custom actions let you create **your own reusable logic** for workflows. This is useful when:
- You need functionality not available in Marketplace.
- You want to share logic across multiple repositories.

---

#### **Types of Custom Actions**
1. **JavaScript Actions**  
   - Runs on Node.js.
   - Great for lightweight tasks.
2. **Docker Actions**  
   - Runs inside a container.
   - Best for complex dependencies.

---

#### ✅ Example: JavaScript Action
Create a folder `my-action` with:
- `action.yml` (metadata)
- `index.js` (logic)

**action.yml**
```yaml
name: "My Custom Action"
description: "Prints a message"
runs:
  using: "node16"
  main: "index.js"
inputs:
  message:
    description: "Message to print"
    required: true
```

**index.js**
```javascript
const core = require('@actions/core');
try {
  const message = core.getInput('message');
  console.log(`Hello from custom action: ${message}`);
} catch (error) {
  core.setFailed(error.message);
}
```

**Usage in workflow:**
```yaml
steps:
  - uses: ./my-action
    with:
      message: "Custom action works!"
```

---

#### ✅ Example: Docker Action
**action.yml**
```yaml
name: "Docker Action"
runs:
  using: "docker"
  image: "Dockerfile"
```

**Dockerfile**
```dockerfile
FROM alpine:latest
RUN apk add --no-cache curl
CMD ["echo", "Hello from Docker Action"]
```

---

#### ✅ Best Practices
- Keep actions **modular and reusable**.
- Pin Node/Docker versions for stability.
- Publish to Marketplace if useful for others.

---

Great question! Let’s clarify **where runners fit in GitHub Actions** and all the related terminologies.

---

### ✅ **What is a Runner?**
- A **runner** is the **machine** that executes your workflow jobs.
- It listens for jobs from GitHub Actions and runs them in its environment.

---

#### **Types of Runners**
1. **GitHub-Hosted Runners**
   - Provided by GitHub.
   - Pre-configured with popular tools (Node, Python, Docker, etc.).
   - OS options: `ubuntu-latest`, `windows-latest`, `macos-latest`.
   - Free minutes included (limits depend on your plan).

2. **Self-Hosted Runners**
   - Your own machine or server.
   - Useful for:
     - Custom environments
     - Private networks
     - Heavy workloads
   - You install the GitHub Actions runner software and register it.

---

### ✅ **Where Does It Run?**
- If you use **GitHub-hosted runners**, jobs run on GitHub’s infrastructure.
- If you use **self-hosted runners**, jobs run on your machine (on-prem or cloud).

---

### ✅ Key Terminologies in GitHub Actions
- **Workflow**: A YAML file defining automation (in `.github/workflows/`).
- **Job**: A set of steps that run on the same runner.
- **Step**: A single task (command or action).
- **Action**: Reusable logic (like Terraform modules).
- **Runner**: The machine executing jobs.
- **Trigger (Event)**: Defines when workflow runs (`on: push`, `pull_request`, etc.).
- **Matrix**: Strategy to run jobs in multiple configurations.
- **Secrets**: Encrypted values for sensitive data.
- **Artifacts**: Files shared between jobs.
- **Cache**: Stores dependencies for faster builds.

---

#### ✅ Example with Runner
```yaml
jobs:
  build:
    runs-on: ubuntu-latest  # GitHub-hosted runner
    steps:
      - uses: actions/checkout@v3
      - run: echo "Running on GitHub-hosted runner"
```

For **self-hosted runner**:
```yaml
jobs:
  build:
    runs-on: self-hosted
    steps:
      - run: echo "Running on my own machine"
```

---

Here’s how you **set up a self-hosted runner step-by-step** and understand where it fits:

---

### ✅ **Step 1: Why Use a Self-Hosted Runner?**
- You need **custom software** or dependencies not available on GitHub-hosted runners.
- You want to run jobs inside a **private network** (e.g., deploy to internal servers).
- You need **more control** over hardware resources.

---

### ✅ **Step 2: Where Will It Run?**
- On **your machine**, **VM**, or **cloud instance** (Linux, Windows, macOS).
- It will **listen for jobs** from GitHub and execute them locally.

---

### ✅ **Step 3: How to Configure a Self-Hosted Runner**
1. **Go to your repository → Settings → Actions → Runners → Add Runner**.
2. Choose **OS** (Linux, Windows, macOS).
3. Download the runner package:
   ```bash
   curl -o actions-runner-linux-x64-<version>.tar.gz https://github.com/actions/runner/releases/latest
   ```
4. Extract and configure:
   ```bash
   tar xzf ./actions-runner-linux-x64-<version>.tar.gz
   ./config.sh --url https://github.com/<owner>/<repo> --token <registration-token>
   ```
5. Start the runner:
   ```bash
   ./run.sh
   ```
   Or install as a service:
   ```bash
   sudo ./svc.sh install
   sudo ./svc.sh start
   ```

---

### ✅ **Step 4: Use It in Workflow**
```yaml
jobs:
  build:
    runs-on: self-hosted
    steps:
      - run: echo "Running on my self-hosted runner"
```
