## Cloud IAM

In this lab, you learn how to perform the following tasks:

* Use Cloud IAM to implement access control
* Restrict access to specific features or resources
* Use the Service Account User role

#### Task 1: Setup for two users

###### Sign in to the Cloud Console as the first user

    gcloud auth login

###### Sign in to the Cloud Console as the second user in another Terminal 

    gcloud auth login

## Task 3: Prepare a resource for access testing

#### Create a bucket and upload a sample file

    gsutil mb -b  gs://my_firstQwiklabs02/

Upload any sample file from your local machine.

    gsutil cp file.txt gs://my_firstQwiklabs02/

When the file has been uploaded, click on the three dots at the end of the line containing the file, and click Rename.

    gsutil mv gs://my_firstQwiklabs02/file.txt gs://my_firstQwiklabs02/sample.txt

#### Verify project viewer access

    gsutil ls

## Task 4: Remove project access

    gsutil iam ch -d user:Username-2:objectViewer gs://my_firstQwiklabs02

## Task 5: Add storage access

    gsutil iam ch  user:Username-2:objectViewer gs://my_firstQwiklabs02

###### Switch to the Username 2 Cloud Console tab.

To view the contents of the bucket you created earlier, run the following command

    gsutil ls gs://my_firstQwiklabs02

## Task 6: Set up the Service Account User

    gcloud iam service-accounts create read-bucket-objects --display-name "read bucket objects"

    gcloud iam service-accounts create read-bucket-objects 

#### Add the user to the service account

    gcloud projects add-iam-policy-binding altostrat.com --member "serviceAccount:read-bucket-objects@someproject.iam.gserviceaccount.com"

## Grant Compute Engine access

    gcloud projects add-iam-policy-binding altostrat.com --member "serviceAccount:read-bucket-objects@someproject.iam.gserviceaccount.com" --role "Compute Engine/Compute Instance Admin (v1)" 
