# Nexus Repository Manager(Registry)

---

## Overview


- **Nexus Repository Manager** is a centralized artifact repository developed by **Sonatype**. It is used to **store, manage, and distribute binary artifacts** and dependencies such as:
    - `.jar`, `.war`, `.ear` files (Java)
    - Docker images
    - npm packages
    - Python packages
    - Helm charts
- **Nexus** is a **repository manager** developed using **Java**.
- It manages **binary artifacts** such as libraries, WAR files, Docker images, etc.
- Acts as a **central hub** for build artifacts during the software lifecycle.
- Open-source and cross-platform (Linux/Windows).

---

## Why Do We Need Nexus?

| Purpose                        | Description                                                                  |
|-------------------------------|------------------------------------------------------------------------------|
| ✅ Central Artifact Hub       | Acts as a single source of truth for build artifacts across teams.           |
| 🔐 Security & Access Control  | Role-based access control over who can publish or retrieve artifacts.        |
| 🚀 CI/CD Efficiency           | Speeds up builds by caching external dependencies locally.                   |
| 🔄 Version Management         | Supports snapshot and release repositories.                                  |
| 📦 Proxy External Repos       | Proxies remote repos like Maven Central, Docker Hub for faster access.       |
| 💾 Binary Storage             | Stores private/custom builds not available in public repositories.           |

---

## How It Fits Into CI/CD Workflow

```
Dev → Git Push → CI Build → Artifact (.war/.jar/.img) → Nexus → Deployment
```

---


# SCM Repositories vs Artifact Repository

---

## 📁 What is an SCM Repository?

**SCM** stands for **Source Code Management**. An SCM repository is used to **store, version, and collaborate on source code**.

### ✅ Features:
- Tracks source code changes over time.
- Supports version control (branches, tags, merges).
- Enables team collaboration.
- Often includes issue tracking, pull requests, etc.

### 🔧 Examples:
- Git (GitHub, GitLab, Bitbucket)
- Subversion (SVN)
- Mercurial

---

## 📦 What is an Artifact Repository?

An Artifact Repository is used to **store compiled or built output artifacts** from source code.

### ✅ Features:
- Stores immutable build outputs.
- Enables artifact promotion (snapshot → release).
- Optimizes CI/CD pipelines.
- Proxies remote repositories (e.g., Maven Central, Docker Hub).

### 🔧 Examples:
- Sonatype Nexus
- JFrog Artifactory
- AWS CodeArtifact
- GitHub Packages
- Docker Hub

---

## 🔁 Key Differences

| Feature                      | SCM Repository (GitHub, GitLab)          | Artifact Repository (Nexus, Artifactory)      |
|------------------------------|------------------------------------------|-----------------------------------------------|
| **Purpose**                  | Manage and version **source code**       | Manage and store **build artifacts**          |
| **Format**                   | Text files (code, configs, markdown)     | Binary files (.jar, .war, .tar.gz, .img)      |
| **Versioning**               | Source code versioning (branches/tags)   | Artifact versioning (semver, snapshot/release)|
| **Common Use**               | Software development & collaboration     | CI/CD pipeline deployment & dependency caching|
| **Example Artifacts**        | `main.java`, `index.js`, `Dockerfile`    | `app.jar`, `my-image:1.0.0`, `npm packages`   |
| **User Interaction**         | Developers write/push/pull code          | CI/CD tools deploy/pull artifacts             |

---



---

## Types of Repositories in Nexus

| Type       | Use Case                                        |
|------------|-------------------------------------------------|
| Hosted     | For internally built artifacts (snapshots/releases) |
| Proxy      | Caches and proxies external repositories         |
| Group      | Combines multiple repositories into a single endpoint |

---

## Alternative Tools to Nexus

| Tool                  | Description                                                                 |
|------------------------|-----------------------------------------------------------------------------|
| **JFrog Artifactory**  | Enterprise-grade repository with broad format support (Java, Go, Docker, etc.) |
| **GitHub Packages**    | GitHub-integrated package and Docker image storage.                        |
| **AWS CodeArtifact**   | Managed AWS artifact repository, IAM-integrated.                           |
| **Azure Artifacts**    | Artifact storage built into Azure DevOps.                                  |
| **Harbor**             | CNCF container registry with RBAC and vulnerability scanning.              |
| **Google Artifact Registry** | Managed Google Cloud repository for multiple artifact types.       |

---

## ✅ When to Use Nexus

Use Nexus when:
- You need a **free** and **self-hosted** solution.
- Your applications are **Java/Maven** based.
- You want to manage both **snapshots and releases** efficiently.
- You want to **proxy** remote repositories like Maven Central.

---

## Repository Setup in Nexus
### Snapshot Repository
- Go to: `Settings → Repositories → Create repository`
- Type: Maven2 (hosted)
- Name: `jio-snapshot`
- Version Policy: `Snapshot`

### Release Repository
- Name: `jio-release`
- Version Policy: `Release`

> Each has a **unique URL**:
```
http://<ip>:port/repository/jio-snapshot/
http://<ip>:port/repository/jio-release/
```

---

## Maven Integration with Nexus
### 1. `pom.xml`
```xml
<distributionManagement>
  <repository>
    <id>nexus</id>
    <url>http://<ip>:port/repository/rushitech-maven-release/</url>
  </repository>
  <snapshotRepository>
    <id>nexus</id>
    <url>http://<ip>:port/repository/rushitech-maven-snapshot/</url>
  </snapshotRepository>
</distributionManagement>
```

### 2. `settings.xml`
```xml
<server>
  <id>nexus</id>
  <username>admin</username>
  <password>password</password>
</server>
```

---

## Common Maven Commands
| Task                              | Command                 |
|-----------------------------------|--------------------------|
| Install to local repo             | `mvn clean install`      |
| Deploy to Nexus snapshot/release | `mvn clean deploy`       |

---

## Snapshot vs Release
| Snapshot                          | Release                          |
|-----------------------------------|----------------------------------|
| Used during continuous development| Used before production deployment|
| Overwritten versions              | Immutable versions               |

---

## Redeploy Same Version
1. Enable "Allow Redeploy" under:
```
Settings → Repository → rushitech-maven-release → Hosted → Allow Redeploy
```
2. Run:
```bash
mvn clean deploy
```

---

## Troubleshooting
### Nexus not starting?
- Ensure:
  - Proper permissions (nexus:nexus, 775)
  - Java installed
  - Running as `nexus` user
  - Logs: `/opt/sonatype-work/nexus3/log/nexus.log`

### Nexus URL not reachable?
- Check:
  - Port 8081 open in EC2 Security Group
  - Nexus service running

---

## Customizations
### Change Nexus Port or Context Path
```bash
vi /opt/nexus/etc/nexus-default.properties
# Modify:
application-port=8081
application-host=0.0.0.0
nexus-context-path=/nexus
sudo systemctl restart nexus
```

### Disable Anonymous Access
- Go to: `Settings → Security → Anonymous Access → Disable`

### Change Admin Password
- Login as `admin`
- Go to: `User settings → Password → Update`
