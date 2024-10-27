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
