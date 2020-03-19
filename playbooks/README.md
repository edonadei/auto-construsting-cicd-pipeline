# Ansible Deploy Apache + Static Website

## Prerequisites:
  - Remote EC2 AMI Source Ubuntu 16.04 LTS
  - Ansible
  - Templates Jinja Apache (ports.conf.j2 & 000-default.conf.j2)

## Vars
- env: Env(dev,hml)
- app_repo: Static HTML Git Public repo Url 
- app_port: Port used to configure Apache

## Execute Play
ansible-playbook -i ubuntu@<Remote_EC2_IP>, --private-key <Key.pem> -e env=<env> app_repo=<app_repo> app_port=<app_port>
