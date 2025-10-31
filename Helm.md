# Helm & Helm with Terraform Learning Roadmap

## 📚 Overview
This guide provides a structured path to learn **Helm** (Kubernetes package manager) and then integrate Helm with **Terraform** for automated Kubernetes deployments.

---

## ✅ Prerequisites
- Basic Kubernetes knowledge
- Familiarity with YAML
- Installed tools:
  - `kubectl`
  - `helm`
  - `terraform`

---

## 🏗 Part 1: Helm Fundamentals

### 1. Introduction to Helm
- What is Helm?
- Why Helm for Kubernetes deployments?
- Helm architecture: Charts, Templates, Values

### 2. Installing Helm
- Installation steps
- Verifying installation
- Helm CLI basics

### 3. Helm Charts
- Chart structure (`Chart.yaml`, `values.yaml`, `templates/`)
- Creating your first Helm chart
- Using official charts from ArtifactHub

### 4. Helm Commands
- `helm create`
- `helm install`
- `helm upgrade`
- `helm rollback`
- `helm uninstall`
- `helm repo add` and `helm repo update`

### 5. Values and Overrides
- Using `values.yaml`
- Passing custom values via CLI
- Environment-specific overrides

### 6. Templating in Helm
- Go templating basics
- Using `{{ .Values }}` and `{{ .Release }}`
- Conditional logic and loops in templates

### 7. Helm Hooks
- Pre-install, post-install hooks
- Use cases for hooks

### 8. Helm Best Practices
- Chart versioning
- Linting charts (`helm lint`)
- Security considerations

---

## 🌍 Part 2: Helm with Terraform Integration

### 1. Why Integrate Helm with Terraform?
- Benefits of managing Helm releases via Terraform
- Use cases in GitOps and CI/CD

### 2. Terraform Helm Provider
- Introduction to `helm` provider in Terraform
- Installing and configuring the provider

### 3. Helm Release Resource
- `helm_release` resource basics
- Syntax and arguments
- Example: Deploying NGINX using Helm via Terraform

### 4. Managing Values in Terraform
- Passing `values.yaml` through Terraform
- Using `set` and `set_sensitive` for overrides

### 5. Dependency Management
- Handling multiple Helm releases in Terraform
- Using `depends_on` for ordering

### 6. State Management
- How Helm releases are tracked in Terraform state
- Best practices for remote state

### 7. Advanced Integration
- Dynamic values using Terraform variables
- Combining Helm with Kubernetes provider
- CI/CD pipeline integration (GitHub Actions, Azure DevOps, etc.)

---

## 🔗 Resources
- [Helm Docs](https://m Helm Provider
- ArtifactHub

---
Great! Let’s start with **Helm basics** and move step by step toward advanced topics and then Terraform.

Here’s the plan for today:

***

### ✅ **Step 1: What is Helm and Why Use It?**

*   Helm is a **package manager for Kubernetes**, similar to how `apt` works for Linux or `npm` for Node.js.
*   It helps you:
    *   **Simplify deployments** by packaging Kubernetes manifests into reusable charts.
    *   **Manage versions** of your application deployments.
    *   **Override configurations easily** using `values.yaml`.

***

#### **Your First Task**

1.  Make sure you have:
    *   A running Kubernetes cluster (Minikube or Docker Desktop works fine).
    *   `kubectl` installed and configured.
    *   Helm installed (`helm version` should work).

2.  Run this command to verify Helm installation:
    ```bash
    helm version
    ```

***

Awesome! Let’s **create your first Helm chart**.

***

### ✅ **Step 2: Create a Helm Chart**

Run this command in your terminal:

```bash
helm create my-first-chart
```

This will generate a folder called `my-first-chart` with the following structure:

    my-first-chart/
    ├── Chart.yaml        # Chart metadata
    ├── values.yaml       # Default configuration values
    ├── charts/           # Dependencies
    └── templates/        # Kubernetes manifests with Go templating

***

### ✅ **Step 3: Understand the Files**

*   **Chart.yaml** → Defines chart name, version, description.
*   **values.yaml** → Holds default values for templates.
*   **templates/** → Contains Kubernetes manifests (Deployment, Service, etc.) with placeholders.

***

### ✅ **Step 4: Install the Chart**

Deploy it to your cluster:

```bash
helm install my-release my-first-chart
```

Check the release:

```bash
helm list
```

***

# Install a chart
helm install <release-name> <chart-path>

# Upgrade a release
helm upgrade <release-name> <chart-path>

# Uninstall a release
helm uninstall <release-name>

# List all releases
helm list
# Show chart details
helm show chart <chart-name>

Great choice! Let’s deep dive into **`values.yaml` and overrides**.

***

### ✅ **Topic 5: Values and Overrides in Helm**

#### **What is `values.yaml`?**

*   It’s the **default configuration file** for your Helm chart.
*   Stores key-value pairs that templates use to render Kubernetes manifests.
*   Example snippet from `values.yaml`:
    ```yaml
    replicaCount: 1

    image:
      repository: nginx
      tag: latest
      pullPolicy: IfNotPresent

    service:
      type: ClusterIP
      port: 80
    ```

***

#### **How Templates Use These Values**

In `templates/deployment.yaml`, you’ll see placeholders like:

```yaml
replicas: {{ .Values.replicaCount }}
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

***

### ✅ **Overriding Values**

You can override defaults in three ways:

1.  **Using `--set` flag**
    ```bash
    helm install my-release my-first-chart --set replicaCount=3
    ```

2.  **Using a custom values file**
    Create `custom-values.yaml`:
    ```yaml
    replicaCount: 3
    image:
      tag: "1.21"
    ```
    Then:
    ```bash
    helm install my-release my-first-chart -f custom-values.yaml
    ```

3.  **Combining multiple files**
    ```bash
    helm install my-release my-first-chart -f dev-values.yaml -f prod-values.yaml
    ```

***

✅ **Pro Tip:**

*   Use **environment-specific values files** (e.g., `values-dev.yaml`, `values-prod.yaml`).
*   Avoid too many `--set` flags; prefer files for maintainability.

***

### Next Topic: **Templating in Helm (Go templates)**

This is where we learn:

*   `{{ .Values }}`, `{{ .Release }}`, `{{ .Chart }}`
*   Conditionals (`if`, `else`)
*   Loops (`range`)
*   Functions (`default`, `quote`, `toYaml`)

***

Here’s a **real-world example** of how you can manage environment-specific overrides using Helm:

***

### ✅ **Scenario: Deploying an App in Dev and Prod**

You have a Helm chart for a web app. The defaults in `values.yaml` look like this:

```yaml
replicaCount: 1

image:
  repository: myapp
  tag: latest

service:
  type: ClusterIP
  port: 80
```

***

#### **Dev Environment Overrides (`values-dev.yaml`)**

```yaml
replicaCount: 1
image:
  tag: "dev"
service:
  type: NodePort
```

***

#### **Prod Environment Overrides (`values-prod.yaml`)**

```yaml
replicaCount: 5
image:
  tag: "stable"
service:
  type: LoadBalancer
```

***

### ✅ **Deploy Commands**

*   **Dev:**
    ```bash
    helm install myapp-dev ./myapp-chart -f values-dev.yaml
    ```
*   **Prod:**
    ```bash
    helm install myapp-prod ./myapp-chart -f values-prod.yaml
    ```

***

### ✅ **Why This Matters**

*   Keeps **base chart generic**.
*   Allows **environment-specific tuning** without duplicating charts.
*   Easy to integrate with **CI/CD pipelines** (e.g., pass `-f values-prod.yaml` during production deploy).

***

Next up: **Templating in Helm**  
We’ll cover:

*   How `{{ .Values }}` works
*   Conditional logic (`if`, `else`)
*   Loops (`range`)
*   Functions like `default`, `quote`, `toYaml`

***

### ✅ **Topic 6: Helm Templating Basics**

Helm uses **Go templates** to dynamically generate Kubernetes manifests. This is what makes Helm powerful.

***

#### **Core Template Objects**

*   `{{ .Values }}` → Access values from `values.yaml`
*   `{{ .Release }}` → Info about the release (name, namespace)
*   `{{ .Chart }}` → Info about the chart (name, version)

Example in `templates/deployment.yaml`:

```yaml
replicas: {{ .Values.replicaCount }}
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

***

#### **Conditionals**

```yaml
{{- if .Values.autoscaling.enabled }}
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
{{- end }}
```

***

#### **Loops**

```yaml
{{- range .Values.configMaps }}
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .name }}
data:
  key: {{ .value }}
{{- end }}
```

***

#### **Functions**

*   `default` → Provide fallback value
    ```yaml
    replicas: {{ default 1 .Values.replicaCount }}
    ```
*   `quote` → Wrap in quotes
*   `toYaml` → Convert object to YAML

***

✅ **Pro Tip:**  
Always run:

```bash
helm template my-release ./my-chart
```

to **render manifests locally** before applying.

***
### ✅ **Topic 7: Helm Hooks**

Helm **hooks** allow you to run certain Kubernetes resources **before or after** specific lifecycle events like `install`, `upgrade`, or `delete`. They’re useful for tasks such as:

*   Running **database migrations** before an app starts.
*   Cleaning up resources after uninstall.
*   Initializing secrets before deployment.

***

#### **Common Hook Events**

*   `pre-install` → Before resources are installed.
*   `post-install` → After resources are installed.
*   `pre-upgrade` / `post-upgrade` → Around upgrades.
*   `pre-delete` / `post-delete` → Around deletion.

***

#### **How to Define a Hook**

Add an annotation in your template:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: db-migration
  annotations:
    "helm.sh/hook": pre-install
    "helm.sh/hook-weight": "0"       # Order of execution
    "helm.sh/hook-delete-policy": before-hook-creation
spec:
  template:
    spec:
      containers:
        - name: migrate
          image: myapp-migrate:latest
      restartPolicy: OnFailure
```

***

#### **Key Annotations**

*   `helm.sh/hook` → Event name (e.g., `pre-install`)
*   `helm.sh/hook-weight` → Execution order (lower runs first)
*   `helm.sh/hook-delete-policy` → When to delete hook resources:
    *   `before-hook-creation`
    *   `hook-succeeded`
    *   `hook-failed`

***

✅ **Pro Tip:**  
Hooks **do not block Helm commands** unless the resource itself takes time (e.g., a Job). Always design hooks to be idempotent.

***

### ✅ **Topic 8: Helm Best Practices**

These practices ensure your Helm charts are **maintainable, secure, and production-ready**:

***

#### **1. Chart Versioning**

*   Update `version` in `Chart.yaml` whenever you make changes.
*   Follow **Semantic Versioning**:
    *   `MAJOR.MINOR.PATCH` (e.g., `1.2.3`)

***

#### **2. Lint Your Charts**

Run:

```bash
helm lint ./my-chart
```

This checks for:

*   Missing fields
*   Invalid YAML
*   Common mistakes

***

#### **3. Use `helm template` Before Deploy**

Render manifests locally:

```bash
helm template my-release ./my-chart
```

This helps catch errors before applying to the cluster.

***

#### **4. Security Considerations**

*   Avoid hardcoding secrets in `values.yaml`.
*   Use **Kubernetes Secrets** or external secret managers.
*   Validate user input when using `--set`.

***

#### **5. Organize Values**

*   Group related configs under sections:
    ```yaml
    image:
      repository: nginx
      tag: latest
    resources:
      limits:
        cpu: 500m
        memory: 256Mi
    ```

***

#### **6. Use `_helpers.tpl`**

*   Define reusable template snippets:
    ```yaml
    {{- define "mychart.fullname" -}}
    {{ .Release.Name }}-{{ .Chart.Name }}
    {{- end -}}
    ```
*   Call with:
    ```yaml
    name: {{ include "mychart.fullname" . }}
    ```

***

### ✅ **Topic 9: Advanced Helm Topics**

#### **1. Chart Dependencies**

*   Define in `Chart.yaml`:
    ```yaml
    dependencies:
      - name: redis
        version: 17.3.11
        repository: https://charts.bitnami.com/bitnami
    ```
*   Update dependencies:
    ```bash
    helm dependency update
    ```

***

#### **2. Helmfile**

*   Manage multiple charts and environments:
    ```yaml
    releases:
      - name: myapp
        chart: ./myapp-chart
        values:
          - values-prod.yaml
    ```

Run:

```bash
helmfile apply
```

***

#### **3. Packaging and Sharing Charts**

*   Package:
    ```bash
    helm package ./my-chart
    ```
*   Push to a repo (e.g., ArtifactHub or private repo).

***

#### **4. Helm in CI/CD**

*   Use `helm upgrade --install` in pipelines.
*   Validate charts with `helm lint` and `helm template`.
*   Store values files per environment in Git.

***

✅ **Helm is now complete!**  
You know:

*   Basics → Advanced
*   Best practices
*   How to integrate in real workflows

***
