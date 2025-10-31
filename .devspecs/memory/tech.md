---
applyTo: "**/*"
---

# Technology Brief: GitHub Actions Observability App

## Core Technologies

1. **GitHub App**: The project will be implemented as a GitHub App to integrate with GitHub repositories and monitor GitHub Actions workflows.
2. **Go**: The programming language used for development.
3. **AWS Lambda**: The hosting platform for the app, ensuring scalability and serverless deployment.
4. **OpenTelemetry SDK**: Used for collecting telemetry data from GitHub Actions workflows.

## Architecture

1. **Event Reception**:

   - GitHub Webhooks will be used to receive events from GitHub workflows.
   - Webhooks will be sent to an API Gateway endpoint.

2. **Pub/Sub Model**:

   - Events received by the API Gateway will be forwarded to an SQS queue.
   - The SQS queue will act as a buffer for reliable and scalable event handling.

3. **Event Processing**:

   - A Lambda function will read events from the SQS queue.
   - The Lambda function will process the events and send telemetry data to the user-configured OpenTelemetry backend.

4. **OpenTelemetry Backend Configuration**:

   - Users must provide the backend URL, credentials, and protocol (gRPC or HTTP) during app installation.
   - No default backend configuration will be provided.

5. **App Instrumentation**:
   - The app will be instrumented using OpenTelemetry to collect its own telemetry data (logs, metrics, traces).
   - The app's telemetry will be sent to a separate OpenTelemetry backend (to be determined).

## Security and Credentials Management

1. **Encryption**:

   - All data will be encrypted in transit using HTTPS.
   - Data stored in DynamoDB will be encrypted at rest using AWS-managed encryption keys.

2. **Configuration Storage**:
   - User-provided configurations (backend URL, credentials, protocol) will be securely stored in DynamoDB.
   - Access to DynamoDB will be restricted using IAM roles.

## Deployment and CI/CD

1. **Deployment**:

   - Terraform will be used for infrastructure as code (IaC) to provision and manage AWS resources.

2. **CI/CD**:
   - GitHub Actions will be used to automate testing, building, and deploying the app.
   - The pipeline will include steps for:
     - Running automated tests.
     - Building and packaging the app.
     - Deploying infrastructure and application code using Terraform.

## Further Research

1. **Event Processing in Lambda**:

   - Determine if additional processing or transformations are needed before sending events to the OpenTelemetry backend.
   - Define error-handling and retry mechanisms for failed event deliveries.

2. **App Telemetry Backend**:
   - Decide on the OpenTelemetry backend for the app's own telemetry data.

## Technical Constraints

- None identified at this time.

## Implementation Patterns

- None identified at this time.
