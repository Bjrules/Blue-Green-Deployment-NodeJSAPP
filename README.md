# Blue-Green-Deployment-NodeJSAPP
**Pratical Demonstration of a Blue-Green Deployment of an Application  The stack used her is nodeJS, ExpressJS, HTML and CSS**
#### This CICD automation project Deploys to kubernetes using the Blue-Green Deployment Release stategy.

**Ensure to read the Jenkinsfile, Dockerfile, README-NodeJSApp.md and Documentation.md to under this project.**   


- Setup Jenkins and EKS on a Machine by installing : Jenkins, Trivy, Docker, AwSCLI & configure it, Terraform, Kubectl and Kubernetes using EKS-Terraform(See the RBAC setup instructions in the RBAC Directory also).

- Setup Sonarqube Server also using Docker : `docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community`

See Blue-Green Deployment part II project

# THANK YOU