# Packer Build Apache AMI
## Requirement
  - Packer
  - Ansible

## Vars
- env: Env(dev,hml)
- app_repo: Static HTML Git Public  repo Url 
- app_name: Application Name
- ec2_ip: EC2 IP where packer is executed to create temporarary SG rule for Remote SSH
- app_port: Port used to configure Apache

## Build
packer build -var env=<env> -var app_repo=<app_repo> -var app_name=<app_name> -var ec2_ip=<ec2_ip> -var app_port=<app_port> buildAMI.json
