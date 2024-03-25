# Jenkins-integration-with-kubernetes
This repository contains the CI/CD Pipeline for kubernetes deployment using Jenkins
# Secrets Handling
The Jenkinsfile_using_vault file uses the Hashicorp Vault to retrieve and use secrets upon:
- Fetching of the source code from the SCM Tool (Github) 
- Pushing of the image in Docker repository


