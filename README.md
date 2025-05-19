# 🚀 Golang Calculator with Multi-Stage Docker Build

This repository demonstrates how to use **multi-stage Docker builds** to create a lean, production-ready Docker image for a simple Golang calculator application. It also highlights the benefits of using **distroless images** and explains why **Ubuntu** is used in the build stage.

---

## 📦 What’s Inside?

This project includes:

- A standard Dockerfile for building a Go app.
- A multi-stage Dockerfile for building and deploying a **minimal**, **secure**, and **efficient** container.
- Explanations of multi-stage builds, image optimizations, distroless containers, and runtime vs. build stages.

---

## 🧱 What is a Multi-Stage Build?

A **multi-stage build** allows you to use multiple `FROM` statements in a single Dockerfile to separate **build-time** and **runtime** environments.

Instead of shipping build tools and compilers in the final image, we build the application in one stage and copy only the necessary output (like a binary) into a minimal base image (e.g., `scratch`).

---

### ✅ Benefits of Multi-Stage Builds

- **Smaller Image Sizes** – Only includes runtime essentials.
- **Better Security** – Fewer packages = smaller attack surface.
- **Optimized Builds** – Stages can be tailored for specific tasks.
- **Efficient Caching** – Speeds up rebuilds when changes are minimal.

---

## 🧪 Dockerfile: Multi-Stage Build Explained

### 🔨 Build Stage

```Dockerfile
FROM ubuntu AS build

RUN apt-get update && apt-get install -y golang-go

ENV GO111MODULE=off

COPY . .

RUN CGO_ENABLED=0 go build -o /app .
```

---

### 🧼 Final Stage (Distroless)

```
FROM scratch

COPY --from=build /app /app

ENTRYPOINT ["/app"]
```

	
 Starts with an empty base (scratch)—no shell, no OS.
	•	Only the compiled binary is copied from the build stage.
	•	The result: a secure, lightweight image ready for deployment.


### 🧊 What Are Distroless Images?

Distroless images contain only application code and runtime dependencies—nothing more.

### 🚀 Why Use Distroless (or scratch)?
	•	Smaller image size = faster downloads and startups
	•	Minimal attack surface = more secure
	•	No shell or package manager = harder for intruders to exploit

scratch is the most minimal distroless base—it’s literally empty.

⸻

### 🧰 Why Use Ubuntu in the Build Stage?

While scratch is great for runtime, we use ubuntu during the build phase because:
	•	It supports installing build tools like golang-go.
	•	Provides a rich environment for customization, debugging, and package management.
	•	Offers flexibility when compiling or configuring large projects.

⸻

### 📏 Image Size Comparison



### 🛠 How to Build and Run

## 🔧 Build the Image

```
docker build -t golang-calculator .
```
## 🚀 Run the Container
```
docker run golang-calculator
```


The compiled Go binary will execute directly in a minimal environment.

⸻

### 📘 Summary

Using multi-stage builds in Docker is a best practice for modern app deployment. It enables developers to:
	•	Ship clean, production-ready containers
	•	Reduce image sizes significantly
	•	Improve performance, scalability, and security
