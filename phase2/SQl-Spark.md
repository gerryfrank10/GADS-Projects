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

    


