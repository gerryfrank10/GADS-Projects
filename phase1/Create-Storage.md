# Google Cloud Fundamentals: Getting Started with Cloud Storage and Cloud SQL

## Objectives

The following are the objectives of the lab
 - Create a Cloud Storage bucket and place an image into it.
 - Create a Cloud SQL instance and configure it.
 - Connect to the Cloud SQL instance from a web server.
 - Use the image in the Cloud Storage bucket on a web page.

## Steps

1. Deploy a web server VM instance

    gcloud compute  instances create bloghost --machine-type=n1-standard-1 --subnet=default --metadata=startup-script="apt-get update&& apt-get install apache2 php php-mysql -y && service apache2 restart"  --tags=http-server --image=debian-9-stretch-v20200910 --image-project=debian-cloud 

2. Create a Cloud Storage bucket using the gsutil command line

    - For convenience, enter your chosen location into an environment variable called LOCATION.

        export LOCATION=US

    - In Cloud Shell, the DEVSHELL_PROJECT_ID environment variable contains your project ID. Enter this command to make a bucket named after your project ID
    
        gsutil mb -l $LOCATION gs://$DEVSHELL_PROJECT_ID

    - Retrieve a banner image from a publicly accessible Cloud Storage location

        gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png

    - Copy the banner image to your newly created Cloud Storage bucket
    
        gsutil cp my-excellent-blog.png gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

    - Modify the Access Control List of the object you just created so that it is readable by everyone
    
        gsutil acl ch -u allUsers:R gs://$DEVSHELL_PROJECT_ID/my-excellent-blog.png

3. Create the Cloud SQL instance

    - Create an sql instace from the command line

        gcloud sql instances create blogdb --tier=db-n1-standard-2 --authorized-networks 35.192.208.2/32

    
    - Create the user and password for the instace blogdb created 

        gcloud sql users set-password root --host=% --instance blogdb --password 12345678
    
4. Configure an application in a Compute Engine instance to use Cloud SQL

    - Ssh to bloghost virtual machine 

        gcloud compute ssh bloghost

    - Change the working directory to that of web server directory 

        cd /var/www/html

    - Use the nano text editor to edit a file called index.php

        sudo nano index.php

    - Paste the contents to these

        ` <html> `
        ` <head><title>Welcome to my excellent blog</title></head> `
        ` <body> `
        ` <h1>Welcome to my excellent blog</h1> `
        ` <?php `
        ` $dbserver = "35.22.12.12"; `
        ` $dbuser = "blogdbuser"; `
        ` $dbpassword = "1234567"; `

        ` $conn = new mysqli($dbserver, $dbuser, $dbpassword);` 
        ` if (mysqli_connect_error()) { `
        `        echo ("Database connection failed: " . mysqli_connect_error());`
        ` } else { `
        `        echo ("Database connection succeeded."); `
        ` } `
        ` ?> `
        ` </body></html> `
    - Save the document and restart the server to confirm the server has really changed

        curl 35.192.208.2/index.php

    - Configure an application in a Compute Engine instance to use a Cloud Storage object

        



