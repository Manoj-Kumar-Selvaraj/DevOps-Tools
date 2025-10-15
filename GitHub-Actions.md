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


