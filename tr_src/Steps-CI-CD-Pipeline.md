# Building Steps to Prepare a CI/CD Pipeline:
- Automation: A "default to automation" mindset—scripting away any manual, repetitive tasks.
- Infrastructure as Code: Managing and build cloud-based self-hosted runners.
- CI/CD Orchestration: Authoring complex multibranch pipelines and managing build agents.
- Security and Compliance Checks - automatically identify vulnerabilities in code and infrastructure.
- Integrate static code analysis tools (e.g., SonarQube) to maintain code quality.
- Monitoring and Logging - ELK Stack (Elasticsearch, Logstash, Kibana):


# Continuous Integration (CI):
_______________________________________________
 
- Docker Build & Scan:
1. GitHub Actions: uses a docker build command and  push docker hub repository
- Runs unit tests 
- GitHub Actions: Implement a vulnerability Image Security Scan. Checks the Docker image for known vulnerabilities (e.g., using Trivy).
- Artifact Repository: Docker image push to Docker Hub or Amazon ECR store .
The image is tagged and pushed to Docker Hub.
-  Uploads the verified image to Docker Hub.


# Continuous Delivery (CD) - Infrastructure Layer (After CI passes)
_______________________________________________
- Test using automated processes
- The code is automatically ready to release and Deploy to production environment at any moment
- Ensure the AWS EC2 environment is ready to receive the code.

- Terraform's Role:
PCreates/updates the rovisioning the VPC, Security Groups (opening port 80/443/22), and the EC2 instance itself.
- Environment Ready	Terraform Passes the EC2 Public IP/DNS back to GitHub Actions via Outputs.


Continuous Deployment (CD) - Application Layer, Deployment (Pull & Run)
_______________________________________________

- Terraform Update the running container on the EC2 instance.
Phase 1:
- Automated Integration Testing:
1. Before the final "push" to production, GitHub Actions can spin up the Docker container in a temporary environment to run end-to-end (E2E) tests.
Phase 2:
- Deployment to EC2:
GitHub Actions connects to your EC2 instance via AWS SSM
Example Command execution:

- `docker pull your-repo/image:latest`
- `docker stop app-container || true`
- `docker rm app-container || true`
- `docker run -d --name app-container -p 80:80 your-repo/image:latest`