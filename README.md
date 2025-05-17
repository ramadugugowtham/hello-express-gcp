# Express API for GCP CI/CD Demo

This project is a simple Express.js application designed to demonstrate a CI/CD pipeline using Google Cloud Build and deployment to Google Cloud Run.

## Features

- A simple Express.js server.
- Two API endpoints:
  - `GET /`: Returns a welcome message.
  - `GET /api/data`: Returns a sample JSON object with a message and a timestamp.
- Unit tests using Jest and Supertest.
- Dockerfile for containerizing the application.
- `cloudbuild.yaml` for configuring Google Cloud Build.
- `.gcloudignore` to exclude unnecessary files from deployment.

## Prerequisites

- Node.js and npm installed.
- Google Cloud SDK (`gcloud`) installed and configured.
- A Google Cloud Project with billing enabled.
- APIs enabled in your GCP project:
  - Cloud Build API
  - Cloud Run API
  - Artifact Registry API (or Container Registry API)

## Project Structure

```
/hello-express
|-- __tests__/
|   |-- index.test.js  # Unit tests for the API
|-- Dockerfile         # Defines the Docker image
|-- cloudbuild.yaml    # Configuration for Google Cloud Build
|-- index.js           # Main Express application file
|-- package.json       # Project metadata and dependencies
|-- package-lock.json  # Exact versions of dependencies
|-- .gcloudignore      # Specifies files to ignore for gcloud commands
|-- README.md          # This file
```

## Local Development

1.  **Clone the repository (if applicable):**

    ```bash
    # git clone <repository-url>
    # cd hello-express-gcp
    ```

2.  **Install dependencies:**

    ```bash
    npm install
    ```

3.  **Run the application:**

    ```bash
    npm start
    ```

    The application will be available at `http://localhost:8080`.

4.  **Run tests:**
    ```bash
    npm test
    ```

## Deployment to Google Cloud

1.  **Replace Placeholders in `cloudbuild.yaml`:**
    Open `cloudbuild.yaml` and replace `[PROJECT_ID]`, `[SERVICE_NAME]`, and `[REGION]` with your actual Google Cloud Project ID, desired service name, and region.

    - `[PROJECT_ID]`: Your Google Cloud project ID.
    - `[SERVICE_NAME]`: A name for your Cloud Run service (e.g., `hello-express-demo`).
    - `[REGION]`: The GCP region where your service will be deployed (e.g., `us-central1`).

    If you are using Artifact Registry instead of Google Container Registry (GCR), update the image paths accordingly:
    `[REGION]-docker.pkg.dev/[PROJECT_ID]/[ARTIFACT_REGISTRY_REPOSITORY_NAME]/[SERVICE_NAME]:$COMMIT_SHA`

2.  **Ensure your Google Cloud Project is set:**

    ```bash
    gcloud config set project [PROJECT_ID]
    ```

3.  **Submit the build to Cloud Build:**
    This command will trigger Cloud Build to build your Docker image, run tests (as defined in `cloudbuild.yaml`), push the image to the registry, and deploy to Cloud Run.

    ```bash
    gcloud builds submit --config cloudbuild.yaml .
    ```

4.  **Access your deployed service:**
    After the build and deployment are successful, Cloud Build will output the URL of your Cloud Run service. You can also find it in the Google Cloud Console under Cloud Run.

## Best Practices Demonstrated

- **Clear Project Structure:** Organized files and directories.
- **Dependency Management:** `package.json` for Node.js dependencies.
- **Containerization:** `Dockerfile` for creating a consistent runtime environment.
- **Automated Testing:** Unit tests run as part of the CI pipeline.
- **CI/CD:** `cloudbuild.yaml` defines an automated build, test, and deploy process.
- **Infrastructure as Code (IaC) principles:** `cloudbuild.yaml` defines the deployment process declaratively.
- **Ignoring unnecessary files:** `.gcloudignore` to optimize uploads.
- **Environment Variables:** `process.env.PORT` for flexible port configuration, essential for Cloud Run.

This setup provides a solid foundation for a CI/CD project on GCP that you can use for your demonstration.
