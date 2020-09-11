# Recommending Products Using Cloud SQL and Spark 

## Objectives
 - Create a Cloud SQL instance
 - Create database tables by importing .sql files from Cloud Storage
 - Populate the tables by importing .csv files from Cloud Storage
 - Allow access to Cloud SQL
 - Explore the rentals data using SQL statements from Cloud Shell

1. Create a Cloud SQL instance

    - Create A Cloud sql instance ` rentals `

        gcloud sql instances create rentals --tier=db-n1-standard-2 --authorized-networks

2. Create the Tables

    - View the details for the Database instance

        gcloud sql instances describe rentals

    - Connect to the database & Enter the password when prompted

        gcloud sql connect rentals --user=root --quiet
    
    - View The database informations

        Show databases;

    - use the following syntax to create the database and tables tables

        ` CREATE DATABASE IF NOT EXISTS recommendation_spark;`

        ` USE recommendation_spark;`

        ` DROP TABLE IF EXISTS Recommendation; `
        ` DROP TABLE IF EXISTS Rating; `
        ` DROP TABLE IF EXISTS Accommodation; `

        ` CREATE TABLE IF NOT EXISTS Accommodation `
        ` ( `
        ` id varchar(255), `
        ` title varchar(255), `
        ` location varchar(255), `
        ` price int, `
        ` rooms int, `
        ` rating float, `
        ` type varchar(255), `
        ` PRIMARY KEY (ID) `
        ` ); `

        ` CREATE TABLE  IF NOT EXISTS Rating `
        ` ( `
        ` userId varchar(255), `
        ` accoId varchar(255), `
        ` rating int, `
        ` PRIMARY KEY(accoId, userId), `
        ` FOREIGN KEY (accoId) `
        `    REFERENCES Accommodation(id) `
        ` ); `

        ` CREATE TABLE  IF NOT EXISTS Recommendation `
        ` ( `
        ` userId varchar(255), `
        ` accoId varchar(255), `
        ` prediction float, `
        ` PRIMARY KEY(userId, accoId), `
        ` FOREIGN KEY (accoId) `
        `    REFERENCES Accommodation(id) `
        ` ); `

        ` SHOW DATABASES;`
    
    - Verify the database created along with the tables 

        show databases `` The recommendation_spark `` Should be seen

        show table; `` Recommendation & Rating & Accommodation `` Tables will also be created


3. Stage data in Cloud Storage

    - Create the buckets for storage 

        echo "Creating bucket: gs://$DEVSHELL_PROJECT_ID"
        gsutil mb gs://$DEVSHELL_PROJECT_ID

        echo "Copying data to our storage from public dataset"
        gsutil cp gs://cloud-training/bdml/v2.0/data/accommodation.csv gs://$DEVSHELL_PROJECT_ID
        gsutil cp gs://cloud-training/bdml/v2.0/data/rating.csv gs://$DEVSHELL_PROJECT_ID

        echo "Show the files in our bucket"
        gsutil ls gs://$DEVSHELL_PROJECT_ID

        echo "View some sample data"
        gsutil cat gs://$DEVSHELL_PROJECT_ID/accommodation.csv


4. Load data from Cloud Storage into Cloud SQL tables

    - Import on instance rentals and import data to recommendation_spark  for accommodations

        gcloud sql instances import rentals gs://$DEVSHELL_PROJECT_ID/accommodation.csv    -D recommendation_spark 

    - Import data on rating to 

        gcloud sql instances import rentals gs://$DEVSHELL_PROJECT_ID/rating.csv -D recommendation_spark


5. Explore The Cloud SQL Data

    If you close the Mysql shell open it up again

    - Check and Query the user data 

        ` USE recommendation_spark; `

        ` SELECT * FROM Rating `
        ` LIMIT 15; `

    - Count the number of rows in the table 

        ` SELECT COUNT(*) AS num_ratings `
        ` FROM Rating; `

    - Calculate the Average review of accommodations

       ` SELECT `
       ` COUNT(userId) AS num_ratings, `
       ` COUNT(DISTINCT userId) AS distinct_user_ratings, `
       ` MIN(rating) AS worst_rating,` 
       ` MAX(rating) AS best_rating,` 
       ` AVG(rating) AS avg_rating `
       ` FROM Rating; `

6. Launch Dataproc

    - gcloud dataproc clusters create rentals --zone=us-central1-a  --master-machine-type=n1-standard-2 --worker-machine-type=n1-standard-2 --subnet default  --image-version 1.3-debian9

![The Result image ](lab-ml.png "The result completion lab")