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
- You want to manage both **snapshots and releases** efficiently.
- You want to **proxy** remote repositories like Maven Central.

---

## Repository Setup in Nexus
### Snapshot Repository
- Go to: `Settings → Repositories → Create repository`
- Type: Maven2 (hosted)
- Name: `rushitech-maven-snapshot`
- Version Policy: `Snapshot`

### Release Repository
- Name: `rushitech-maven-release`
- Version Policy: `Release`

> Each has a **unique URL**:
```
http://<ip>:port/repository/rushitech-maven-snapshot/
http://<ip>:port/repository/rushitech-maven-release/
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

 Nexus Proxy Repository with Maven Setup

How to configure a **Nexus Proxy Repository** for **Maven Central** and set up Maven to use it.

---

## What is a Nexus Proxy Repository?

A **Proxy Repository** in Sonatype Nexus acts as a **caching proxy** for another repository (e.g., Maven Central). It:
- Saves bandwidth
- Speeds up builds
- Provides a cache of artifacts

---

## Step-by-Step: Setup Nexus Proxy Repository for Maven Central

### Step 1: Login to Nexus
1. Open Nexus UI: `http://<your-nexus-host>:8081`
2. Login with admin credentials

---

### Step 2: Create Proxy Repository
1. Go to: **Repositories** → **Create repository**
2. Choose: `maven2 (proxy)`
3. Fill in details:
   - **Name**: `maven-central`
   - **Remote storage**: `https://repo1.maven.org/maven2/`
   - **Blob store**: `default`
   - **Version Policy**: `Release`
   - **Layout Policy**: `Strict`
   - **HTTP/HTTPS**: Enable one or both
4. Click **Create Repository**

---

### Step 3: Configure Maven to Use Nexus Proxy

You need to update your `~/.m2/settings.xml` or $M2_HOME/conf/settings.xml file to use the proxy.

#### Example `settings.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                              https://maven.apache.org/xsd/settings-1.0.0.xsd">
  <mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://NEXUSIPORHOSTNAME:8081/repository/maven-central/</url>
    </mirror>
  </mirrors>

  <profiles>
    <profile>
	
      <id>nexus</id>
	  
      <repositories>
        <repository>
          <id>central</id>
          <url>https://repo1.maven.org/maven2/</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </repository>
      </repositories>
	  
      <pluginRepositories>
        <pluginRepository>
          <id>central</id>
          <url>https://repo1.maven.org/maven2</url>
          <releases><enabled>true</enabled></releases>
          <snapshots><enabled>false</enabled></snapshots>
        </pluginRepository>
      </pluginRepositories>
	  
    </profile>
	
  </profiles>

  <activeProfiles>
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
  
</settings>
```

> Replace `<your-nexus-host>` with your Nexus server IP or domain.

---

## Verify the Setup

1. Run any Maven command (e.g., `mvn clean install`)
2. Nexus should show artifact downloads cached in the **Browse** section under `maven-central`


---

## Section-wise Breakdown

###  `<mirrors>` Configuration

```xml
<mirrors>
  <mirror>
    <id>nexus</id>
    <mirrorOf>*</mirrorOf>
    <url>http://<your-nexus-host>:8081/repository/maven-central/</url>
  </mirror>
</mirrors>
```

**What it does:**

- Redirects all Maven repository requests (`mirrorOf="*"`) to Nexus.
- Maven will fetch dependencies from:
  ```
  http://<your-nexus-host>:8081/repository/maven-central/
  ```
- Nexus will serve cached content or download it from the actual Maven Central and cache it.

---

###  `<profiles>` – Repository Definitions

```xml
<profiles>
  <profile>
    <id>nexus</id>
    <repositories>
      <repository>
        <id>central</id>
        <url>https://repo1.maven.org/maven2/</url>
        <releases><enabled>true</enabled></releases>
        <snapshots><enabled>false</enabled></snapshots>
      </repository>
    </repositories>
  </profile>
</profiles>
```

**What it does:**

- Defines a repository named `central` (Maven's default).
- Serves as a placeholder since the actual fetching happens via the mirror.
- Snapshots are disabled unless required.

---

###  `<activeProfiles>` – Activate the Nexus Profile

```xml
<activeProfiles>
  <activeProfile>nexus</activeProfile>
</activeProfiles>
```

**What it does:**

- Activates the `nexus` profile so Maven includes the custom repository settings during builds.

---


###  `<pluginRepositories>` for Maven Plugins

Maven also downloads plugins (e.g., `maven-compiler-plugin`, `maven-surefire-plugin`) from remote repositories. Configure plugin repositories as shown:

```xml
<pluginRepositories>
  <pluginRepository>
    <id>central</id>
    <url>https://repo1.maven.org/maven2</url>
    <releases><enabled>true</enabled></releases>
    <snapshots><enabled>false</enabled></snapshots>
  </pluginRepository>
</pluginRepositories>
```
---

## Summary

| Step | Action |
|------|--------|
| 1 | Create a Maven proxy repo in Nexus |
| 2 | Point `settings.xml` to the Nexus proxy repo |
| 3 | Use Maven as usual — Nexus caches the dependencies |

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
