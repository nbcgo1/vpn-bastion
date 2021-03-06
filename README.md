# vpn-bastion
Deployment of Bastion Instance accessible via VPN

VPC + VPN Provisioning
======================

This project creates (and delete) Linux Server Bastion in its own isolated VPC in AWS.
It adds a VPN Server compabible with iOS and MacOS native VPN Client provides access the Linux Server when you hit the road.
The output provides DNS Names, IP addresses and Credentials for the VPN Settings.
API Gateway provides a way to start this bastion only when needed.

Requirements
------------

- An SSH Public Key

Define all the Values in terraform.tfvars as shown in terraform.tfvars.example

If you don't want to use DNS Names with Route 53 but only IP, delete the following file:
- vpn-server-dnsname.tf

Otherwise you will need:
- A zone ID for your DNS Zone in Route 53

Optional to store terraform states in S3:
- backend.safe.tf copied from backend.safe.tf.example

Dependencies
------------

ipv6=false doesn't work yet in current version
Bug Issue 688:
 - https://github.com/terraform-providers/terraform-provider-aws/issues/688
 - https://github.com/terraform-providers/terraform-provider-aws/pull/2103


Usage Example
----------------

- Create terraform.tfvars from terraform.tfvars.example
- (optional) Create backend.safe.tf from backend.safe.tf.example
- terraform init
- (optional) terraform workspace new <workspace_name>
- terraform apply
- terraform destroy
- (optional) terraform workspace delete <workspace_name>

You can use inline vars to overide terraform.tfvars and deploy in a different region
- terraform workspace new paris
- terraform apply -var vpcname='VPN-Paris' -var region='eu-west-3' -var hostname='vpn-paris.domain.com'
- terraform destroy -var vpcname='VPN-Paris' -var region='eu-west-3' -var hostname='vpn-paris.domain.com'
- terraform workspace delete paris


To Do
-----

- Create API Gateway endpoints in order to start the instance only when needed from my Smartphone App.
- Replace Terraform States storage in S3 backend by Consul or equivalent
- Enable IPv6 VPN (MacOS and iOS clients don't support IPv6)
