# App Dev: Setting up a Development Environment v1.1 

## Objectives

The Following are the objectives of the lab

 - Provision a Google Compute Engine instance
 - Connect to the instance using SSH
 - Install software on the instance
 - Verify the software installation

## Steps

1. Creating a Compute Engine Virtual Machine Instance

    - Create a virtual machine instance from gcloud, with firewall rule allowing http
        gcloud compute  instances create  dev-instance --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud --zone=us-central1-a