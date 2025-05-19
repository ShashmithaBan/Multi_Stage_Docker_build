🚀 Golang Calculator with Multi-Stage Docker Build

This repository demonstrates how to use multi-stage Docker builds to create a lean, production-ready Docker image for a simple Golang calculator application. It also highlights the benefits of using distroless images and explains why Ubuntu is used in the build stage.

⸻

📦 What’s Inside?

This project includes:
	•	A standard Dockerfile for building a Go app.
	•	A multi-stage Dockerfile for building and deploying a minimal, secure, and efficient container.
	•	Explanations of multi-stage builds, image optimizations, distroless containers, and runtime vs. build stages.

⸻

🧱 What is a Multi-Stage Build?

A multi-stage build allows you to use multiple FROM statements in a single Dockerfile to separate build-time and runtime environments.

Instead of shipping build tools and compilers in the final image, we build the application in one stage and copy only the necessary output (like a binary) into a minimal base image (e.g., scratch).

⸻

✅ Benefits of Multi-Stage Builds
	•	Smaller Image Sizes – Only includes runtime essentials.
	•	Better Security – Fewer packages = smaller attack surface.
	•	Optimized Builds – Stages can be tailored for specific tasks.
	•	Efficient Caching – Speeds up rebuilds when changes are minimal.

⸻

🧪 Dockerfile: Multi-Stage Build Explained

🔨 Build Stage
FROM ubuntu AS build

RUN apt-get update && apt-get install -y golang-go

ENV GO111MODULE=off

COPY . .

RUN CGO_ENABLED=0 go build -o /app .
