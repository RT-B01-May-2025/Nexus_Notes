# 📦 What is an Artifact Registry?

An **Artifact Registry** is a **managed service or system** used to store and serve **build artifacts**, such as:

- Docker images
- JAR/WAR files
- npm packages
- Python packages
- Helm charts
- Other compiled binaries (e.g., .zip, .tar.gz)

It serves as a **central location** for versioned, secure, and repeatable deployments in CI/CD pipelines.

---

## 🔧 Key Features

| Feature                      | Description                                                       |
|------------------------------|-------------------------------------------------------------------|
| ✅ **Storage of Binaries**   | Stores compiled application code, containers, libraries, etc.     |
| 📌 **Versioning**            | Keeps track of different versions of the same artifact            |
| 🔐 **Access Control**        | Role-based access for publishing and downloading                  |
| 🚀 **CI/CD Integration**     | Integrates with build tools and pipelines                         |
| 🔁 **Caching/Proxy**         | Can cache external repositories (like Maven Central, Docker Hub)  |
| 📦 **Multi-format Support**  | Supports Maven, npm, Docker, Python, etc.                         |

---

## 🧠 Common Use Cases

- Deploying Docker images from a private registry
- Publishing Maven/NPM packages during builds
- Promoting artifacts from dev to staging to prod
- Reusing libraries without fetching from public internet

---

## 🛠️ Popular Artifact Registries

| Tool/Service                | Type              | Notes                                |
|----------------------------|-------------------|--------------------------------------|
| **Google Artifact Registry** | Cloud-managed     | Supports Docker, Maven, npm, PyPI    |
| **AWS CodeArtifact**         | Cloud-managed     | Supports Maven, npm, Python, NuGet   |
| **JFrog Artifactory**        | Self-hosted/SaaS  | Enterprise-grade, multi-format       |
| **Sonatype Nexus**           | Self-hosted/OSS   | Supports Maven, npm, Docker, etc.    |
| **GitHub Packages**          | Cloud-managed     | Integrated with GitHub workflows     |
| **Docker Hub**               | Cloud-managed     | Focused on Docker images             |

---

## 📌 Summary

> **Artifact Registry** is a critical part of modern DevOps and CI/CD pipelines. It ensures that build artifacts are versioned, secured, and readily available for automated deployments.

---

# 📦 What is a Repository in a Registry?

In the context of an **Artifact Registry** (such as Docker Hub, Nexus, Artifactory, or Google Artifact Registry), a **repository** is a **logical namespace** used to organize and store **artifacts** (like Docker images, JARs, npm packages, etc.).

---

## 🧱 Analogy

- **Registry** → A library
- **Repository** → A bookshelf in that library
- **Artifact** → A book on the shelf

---

## 🔍 Definition

> A **repository** in a registry is a collection of versioned artifacts grouped by project, type, or use case.

---

## 📂 Types of Repositories

| Repository Type     | Artifact Format     | Example Artifacts                     |
|---------------------|---------------------|----------------------------------------|
| **Docker**          | Docker images       | `myapp:1.0`, `nginx:latest`            |
| **Maven**           | Java JAR/WAR files  | `com.example:my-app:1.0.0`             |
| **npm**             | Node.js packages    | `@myorg/utils@1.2.3`                   |
| **PyPI**            | Python packages     | `requests-2.28.1-py3-none-any.whl`     |
| **Helm**            | Helm charts         | `nginx-chart-1.0.0.tgz`                |

---

## 🔐 Repository Roles in a Registry

- Logical **grouping** of artifacts
- Fine-grained **access control** (RBAC)
- **Versioning** and artifact lifecycle management
- Support for **promotion flows** (dev → stage → prod)

---

## 🧰 Real-world Examples

### Google Artifact Registry
```
Location: asia-south1
Registry: my-project-docker
Repository: backend-api
Artifact: backend-api:1.0.2
```

### Docker Hub
```
Registry: docker.io
Repository: myusername/myapp
Tags: latest, v1.0, v1.1
```

---

## ✅ Summary

- A **repository** is a storage unit **within a registry** for organizing, storing, and versioning build artifacts.
- Each repository is usually tied to:
  - A **format** (Docker, Maven, npm, etc.)
  - A **purpose/project/team**
  - Specific **access permissions**
