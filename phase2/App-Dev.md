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

    - Connect to the vm using ssh
        gcloud compute ssh dev-instance

2. Install software on the VM instance

    - Update the system and install git
        sudo apt update && sudo apt install git 

    - Download nodejs 
        curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

    - Install npm and nodejs 
        sudo apt install nodejs

3. Configure the VM to Run Application Software

    - check the version of Node.js, execute the following command

        node -v

    - Clone the repositiiory from GoogleCloudPlatform 

        git clone https://github.com/GoogleCloudPlatform/training-data-analyst

    - Change the working directory to that of GoogleCloudPlatform 

        cd ~/training-data-analyst/courses/developingapps/nodejs/devenv/

    - Run the simple nodejs app 

        sudo node server/app.js 

    - Check the status of the app via Curl

        curl http://localhost
        You should see this message ` Hello GCP dev `

    - Install nodejs library

        npm install

    - Run a nodejs app that lists compute-engine instances

        node list-gce-instances.js
