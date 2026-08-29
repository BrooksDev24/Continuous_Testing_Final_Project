# QR Code Generator with CI/CD Pipeline

A Node.js/Express web app that generates downloadable QR codes from any URL, built as the final project for a Continuous Testing course. The focus of this project is less about the app itself and more about the automated CI/CD pipeline wrapped around it — covering containerized deployment, security scanning, and code quality analysis.

## What it does

- Enter a URL in the browser, and the app generates a QR code image for it (rendered with EJS templates).
- Each generated QR code is saved as a PNG in the `store/` directory with a timestamped filename.
- A `/download` route lets you retrieve the saved PNG file directly.

## Tech Stack

- **Backend:** Node.js, Express
- **Templating:** EJS
- **QR Generation:** `qrcode` npm package
- **Containerization:** Docker, Docker Compose
- **CI/CD:** Jenkins

## CI/CD Pipeline

The included `Jenkinsfile` automates the full build-to-deploy process:

1. **Build & Tag** — builds a Docker image and tags it for release
2. **Push to Docker Hub** — publishes the built image
3. **Deploy** — tears down and redeploys the container via `docker-compose`
4. **DAST Scan (OWASP ZAP)** — runs a dynamic security scan against the live deployed app
5. **Vulnerability Scan (Trivy)** — scans the container image for high/critical CVEs and fails the build if found
6. **SCA/SAST (Snyk)** — checks dependencies and source code for known vulnerabilities
7. **Code Quality (SonarQube)** — static analysis for maintainability and code smells

## Running Locally

**With Docker:**

```bash
docker-compose up -d
```

The app will be available at `http://localhost` (mapped to port 3000 in the container).

**Without Docker:**

```bash
npm install
node index.js
```

The app runs on `http://localhost:3000` by default.

## What I Learned

This project was primarily an exercise in building a real DevSecOps pipeline around a small application — going beyond "does it build and deploy" to include automated security and quality gates (SAST, DAST, dependency scanning, and vulnerability scanning) that would be expected in a production CI/CD workflow.
