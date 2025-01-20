# Job Portal Backend with Automated Deployment and Infrastructure as Code

## Project Overview
This project is a **Job Portal Backend** application, providing a platform for employers and job seekers to connect efficiently. Employers can post jobs, review applications, and manage job postings, while job seekers can search for jobs, apply to them, and receive email notifications for opportunities matching their preferences. The backend is built with Node.js and MongoDB, leveraging modern cloud and DevOps practices for scalability and reliability.

---

## Features

### Backend Features
- **Authentication**: Users can authenticate as either employers or job seekers.
- **Email Verification**: Registration and critical actions are secured with email OTP verification using Nodemailer.
- **Job Management**:
  - Employers can post jobs, update job details, and manage applications.
  - Job seekers can apply for jobs, update their profiles, and search for jobs using filters.
- **Notifications**: Cron jobs automatically send email notifications to job seekers when relevant job postings are made.
- **Role-Based Actions**:
  - Employers can approve or reject applications.
  - Job seekers can track their applications.

### Infrastructure Features
- **Highly Available Architecture**: The application is hosted in a VPC spanning two availability zones with separate public and private subnets.
- **Secure and Scalable Deployment**:
  - ECS Fargate Cluster running containers in private subnets.
  - Application Load Balancer (ALB) in public subnets to route traffic securely.
  - Auto-scaling enabled for ECS tasks based on CPU usage.
- **SSL/TLS and DNS**: Enforced HTTPS traffic with certificates from AWS ACM. Traffic is redirected from HTTP (port 80) to HTTPS (port 443).
- **Database**: MongoDB Atlas, allowing secure communication from private subnets via a NAT Gateway.
- **Environment Variables**: Secrets managed using AWS Secrets Manager and accessed dynamically at runtime.
- **CI/CD Pipeline**: Automated build, push, and deployment using GitHub Actions integrated with Amazon ECR and ECS.

---

## Architecture

### Key Components
1. **Virtual Private Cloud (VPC)**:
   - Two availability zones with public and private subnets in each.
   - **Public Subnets**: Host the ALB.
   - **Private Subnets**: Host ECS Fargate tasks and ensure no direct external access.

2. **NAT Gateway**:
   - Deployed in public subnets to allow ECS tasks in private subnets to access external resources like MongoDB Atlas and external APIs.

3. **Application Load Balancer (ALB)**:
   - Routes incoming traffic to ECS tasks.
   - Configured to enforce HTTPS with an ACM-managed SSL certificate.
   - Health checks configured to monitor container health.

4. **Target Group**:
   - Includes ECS tasks running in private subnets.
   - Monitored using health checks from the ALB.

5. **ECS Fargate Cluster**:
   - Hosts containerized services with auto-scaling based on CPU utilization.
   - Integrated with Amazon ECR for container images.

6. **Amazon ECR**:
   - Stores Docker images built during CI/CD workflows.

7. **Security Groups**:
   - ALB Security Group: Allows inbound traffic on ports 80 and 443.
   - ECS Security Group: Allows only ALB traffic to access the containers.

8. **Secrets Management**:
   - AWS Secrets Manager stores sensitive environment variables.
   - Secrets are fetched dynamically during runtime using AWS SDK.

### Diagram
![Alt Text](https://github.com/yashGoyal40/jobportal-Ansible-Iac/blob/main/diagram.png)


---

## CI/CD Pipeline
The deployment pipeline is automated using GitHub Actions:
1. **Code Push**: Triggered on pushes to the `main` branch.
2. **Build**: Docker images are built using the application code.
3. **Push to ECR**: Images are tagged and pushed to Amazon ECR.
4. **Deployment**: ECS service is updated with the new image, and a new deployment is triggered.

GitHub Repository: [Job Portal Backend CI/CD](https://github.com/yashGoyal40/containrerised-job-portal-with-automation)

---

## Setup and Deployment

### Prerequisites
- AWS CLI configured with sufficient permissions.
- Ansible installed for infrastructure automation.
- MongoDB Atlas for database hosting.
- A domain name (optional for ALB configuration with custom DNS).

### Steps
1. **Infrastructure Provisioning**:
   - Use Ansible playbooks to provision:
     - VPC with subnets.
     - NAT Gateway, ALB, ECS cluster, target group, and security groups.
     - ECR repository and IAM roles.
   - Ensure ACM SSL certificates are requested and validated.

2. **Code Deployment**:
   - Push application code to the `main` branch.
   - GitHub Actions build and push the Docker image to ECR.
   - ECS service is updated, and tasks are deployed.

3. **DNS and SSL Setup**:
   - Configure your domain to point to the ALB using Route 53 or your domain registrar.
   - ACM certificates automatically secure traffic.

---

## Future Enhancements
- Integrate monitoring tools like CloudWatch, Prometheus, or Grafana.
- Add automated testing workflows to the CI/CD pipeline.
- Implement caching mechanisms like Redis for better performance.
- Introduce a frontend for the job portal.

---

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for suggestions or improvements.

---

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

## Contact
For any inquiries or issues, feel free to contact [Yash Goyal/yahsg4118@gmail.com].

