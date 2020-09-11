# Google Cloud Fundamentals: Getting Started with Compute Engine

## Lab1 

The following are the objectives of the lab
 - Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console
 - Create a Compute Engine virtual machine using the gcloud command-line interface.
 - Connect between the two instances.

## steps 
1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console
    ### Create a firewall rule first 
    gcloud compute firewall-rules create allow-http --action=ALLOW --destination=INGRESS --rules=http:80 --target-tags=http

    ### Then create an instance associated with the firewall rule
    gcloud compute instances create my-vm-1 --machine-type 'n1-standard-1' --image-project 'debian-cloud' --image 'debian-9-stretch-v20200910' --subnet 'default' --tags=http

    

2. Create a Compute Engine virtual machine using the gcloud command-line interface.

    gcloud compute instances create my-vm-2 --machine-type 'n1-standard-1' --image-project 'debian-cloud' --image 'debian-9-stretch-v20200910' --subnet 'default'
3. Connect Between the two instances 
    - Connect to my-vm-2
        gcloud compute ssh my-vm-2
    
    - ping my-vm-1 from my-vm-2
        ping -c 2 my-vm-1

    - Use ssh to open the terminal at my-vm-1 from my-vm-2
        ssh my-wm-1

    - At my-vm-1, now install nginx web server
        sudo apt update
        sudo apt install nginx -y

    - Edit The contents of the default nginx web page
        sudo nano /var/www/html/index.nginx-debian.html

    - Add a text like this replace YOUR_NAME with your name
        Hi from gerald

    - Exit at the nano editor and confirm that the webserver is running, at the command prompt
        curl http://localhost/
    
    - Exit at my-vm-1 terminal with ` exit ` command and confirm you can see the web server from my-vm-2
        curl http://my-vm-1/

    
