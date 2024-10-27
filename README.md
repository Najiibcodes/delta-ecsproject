TM App - Open Source App Hosted on ECS with Terraform 🚀
========================================================

This project deploys the TM App, based on Amazon's [Threat Composer Tool](https://github.com/aws-samples/threat-composer), an open-source application for threat modeling and security assessments. The app is securely hosted at [https://najiib.co.uk](https://najiib.co.uk) on AWS ECS using Terraform, featuring an HTTPS-enabled architecture with AWS-managed resources for scalability, security, and resilience.

Project Overview
----------------

The deployment is fully automated using Infrastructure as Code (IaC) with Terraform and follows containerization best practices with Amazon Elastic Container Registry (ECR) and Elastic Container Service (ECS). A GitHub Actions CI/CD pipeline handles continuous integration and deployment for a seamless workflow.

### Key Features

*   **Automated Infrastructure**: Terraform modules provision and manage AWS resources.
    
*   **Containerized Deployment**: App image built and stored in ECR.
    
*   **HTTPS Security**: Secure access to the app via SSL/TLS using ACM.
    
*   **Scalable Architecture**: Deployed on ECS with an Application Load Balancer (ALB) for load balancing.
    

Infrastructure Overview
-----------------------

This project utilizes the following AWS resources:

*   **Amazon ECS**: Manages the TM app's containerized services.
    
*   **ALB (Application Load Balancer)**: Distributes HTTPS traffic securely to ECS tasks.
    
*   **Amazon Route 53**: Manages DNS and domain routing.
    
*   **AWS Certificate Manager (ACM)**: Manages SSL certificates for HTTPS.
    

### Project Architecture

The infrastructure consists of a custom VPC, ECS Cluster, Security Groups, and ACM for TLS. Route 53 configures DNS to route traffic securely to the TM App hosted on ECS. (Refer to architecture\_diagram.png for details.)

Deployment Instructions
-----------------------

### Prerequisites

*   **AWS CLI** with necessary permissions
    
*   **Docker** for local containerization
    
*   **Terraform** to manage IaC
    

### Deployment Steps

1.  bashCopy codegit clone cd
    
2.  bashCopy codedocker build -t tm-app .docker tag tm-app:latest /tm-app:latestdocker push /tm-app:latest
    
3.  bashCopy codeterraform initterraform apply
    
4.  **Access Application**:Visit [https://najiib.co.uk](https://najiib.co.uk) to access the live TM App.
    

Local Development
-----------------

1.  bashCopy codeyarn install
    
2.  bashCopy codeyarn buildserve -s build
    
3.  **Access Locally**:Open [http://localhost:3000/workspaces/default/dashboard](http://localhost:3000/workspaces/default/dashboard) to view the app locally.
    

CI/CD Pipeline
--------------

The GitHub Actions workflow automates CI/CD for this project. The main jobs include:

*   **Docker Build & Push**: Builds and deploys images to ECR on main branch updates.
    
*   **Terraform Apply**: Applies infrastructure changes upon updates.

### Architecture Diagram 🚀

```mermaid
flowchart TD
    %% GitHub Actions CI/CD Pipeline with Local Testing
    A["Create Repository"]
    B["Checkout Code"]
    C["Secrets Setup (AWS Access Key, Secret Key, Region)"]
    D["Build Docker Image"]
    E["Test Workflow Locally with Act"]
    F["Push to ECR (Elastic Container Registry)"]

    subgraph CI/CD Pipeline
        A --> B --> C --> D --> E --> F
    end

    %% Terraform Resources
    G{"Deploy with Terraform"}
    
    subgraph Terraform Modules
        H["VPC Module (CIDR: 10.0.0.0/16)"]
        I["Subnets Module (eu-west-2a & eu-west-2b)"]
        J["Security Groups Module (ECS Security Group)"]
        K["ALB Module (HTTPS Load Balancer)"]
        L["ECS Cluster Module (tm-cluster)"]
        M["Route 53 DNS Module (Domain Mapping)"]
    end
    
    %% ECS & Load Balancer Deployment
    N["ECS Task Definition (Family: threatmodel-task)"]
    O["Application Load Balancer (HTTPS Endpoint)"]

    %% Connections and Dependencies
    F --> G
    G --> H
    H --> I
    I --> J
    G --> K --> O
    G --> L --> N
    O --> M

    %% Endpoints
    M --> P["App Live on HTTPS Domain: www.teamdelta.com"]

%% Node Styling
    style A color:#FFFFFF, fill:#007ACC, stroke:#007ACC
    style B color:#FFFFFF, fill:#1E88E5, stroke:#1E88E5
    style C color:#FFFFFF, fill:#8E24AA, stroke:#8E24AA
    style D color:#FFFFFF, fill:#43A047, stroke:#43A047
    style E color:#FFFFFF, fill:#795548, stroke:#795548
    style F color:#FFFFFF, fill:#FF6F00, stroke:#FF6F00
    style G color:#FFFFFF, fill:#6A1B9A, stroke:#6A1B9A
    style H color:#FFFFFF, fill:#009688, stroke:#009688
    style I color:#FFFFFF, fill:#4CAF50, stroke:#4CAF50
    style J color:#FFFFFF, fill:#FFC107, stroke:#FFC107
    style K color:#FFFFFF, fill:#FF5722, stroke:#FF5722
    style L color:#FFFFFF, fill:#3F51B5, stroke:#3F51B5
    style M color:#FFFFFF, fill:#CDDC39, stroke:#CDDC39
    style N color:#FFFFFF, fill:#039BE5, stroke:#039BE5
    style O color:#FFFFFF, fill:#AA00FF, stroke:#AA00FF
    style P color:#FFFFFF, fill:#D32F2F, stroke:#D32F2F
