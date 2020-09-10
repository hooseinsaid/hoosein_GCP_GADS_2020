# AK8S-01 Accessing the GCP Console and Cloud Shell

In this lab, you become familiar with the GCP web-based interface. Two integrated environments are available:

In this lab, you learn how to perform the following tasks:

1. Learn how to access the GCP Console and Cloud Shell

2. Become familiar with the GCP Console

3. Become familiar with Cloud Shell features, including the Cloud Shell code editor

4. Use the GCP Console and Cloud Shell to create buckets and VMs and service accounts

5. Perform other commands in Cloud Shell

## Task 1. Explore the GCP Console

 create bucket storage
  gsutil mb gs://qwiklabs-gcp-00-929cb24cea68

## Create a virtual machine (VM) instance

    gcloud beta compute --project=qwiklabs-gcp-04-8d95dde77632 instances create first-vm --zone=us-central1-c --machine-type=e2-micro --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=850855358212-compute@developer.gserviceaccount.com --tags=http-server --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=first-vm --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

#### Create  firewall-rules

    gcloud compute --project=qwiklabs-gcp-04-8d95dde77632 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

## create service account

    gcloud iam service-accounts create sa-name \
    --description="sa-description" --display-name="sa-display-name"

## Create a second Cloud Storage bucket and verify it in the GCP Console

create bucket storage

    gsutil mb gs://qwiklabs-gcp-00-929cb24cea70

## Task 2

##### Use Cloud Shell to set up the environment variables for this task

 In Cloud Shell, use the following commands to define the environment variables used in this task

    MY_BUCKET_NAME_1=[BUCKET_NAME]
    MY_BUCKET_NAME_2=[BUCKET_NAME_2]
    MY_REGION=us-central1
##### Create a second Cloud Storage bucket and verify it

    gsutil mb gs://$MY_BUCKET_NAME_2

## Task 3 create a second virtual machine

1. In Cloud Shell, execute the following command to list all the zones in a given region:

        gcloud compute zones list | grep $MY_REGION

2. Select a zone from the first column of the list. Notice that GCP zones' names consist of their region name, followed by a hyphen and a letter.

    Store your chosen zone in an environment variable.

        MY_ZONE=[ZONE]
    Set this zone to be your default zone

        gcloud config set compute/zone $MY_ZONE
    store a name in an environment variable you will use to create a VM

        MY_VMNAME=second-vm
    Create a VM in the default zone that you set earlier in this task using the new environment variable to assign the VM name.

        gcloud compute instances create $MY_VMNAME \
        --machine-type "n1-standard-1" \
        --image-project "debian-cloud" \
        --image-family "debian-9" \
        --subnet "default"
    List the virtual machine instances in your project.

        gcloud compute instances list

#### create a second service account

execute the following command to create a new service account:

    gcloud iam service-accounts create test-service-account2 --display-name "test-service-account2"

execute the following command to grant the second service account the Project viewer role:

    gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:test-service-account2@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --role roles/viewer

## Task 4 Work with Cloud Storage
Copy a picture of a cat from a Google-provided Cloud Storage bucket to your Cloud Shell.

    gsutil cp gs://cloud-training/ak8s/cat.jpg cat.jpg

Copy the file into one of the buckets that you created earlier.

    gsutil cp cat.jpg gs://$MY_BUCKET_NAME_1
Copy the file from the first bucket into the second bucket:

    gsutil cp gs://$MY_BUCKET_NAME_1/cat.jpg gs://$MY_BUCKET_NAME_2/cat.jpg

#### Set the access control list for a Cloud Storage object

To get the default access list that's been assigned to cat.jpg

    gsutil acl get gs://$MY_BUCKET_NAME_1/cat.jpg  > acl.txt

    cat acl.txt
To change the object to have private access, execute the following command:

    gsutil acl set private gs://$MY_BUCKET_NAME_1/cat.jpg
To verify the new ACL that's been assigned to cat.jpg, execute the following two commands:

    gsutil acl get gs://$MY_BUCKET_NAME_1/cat.jpg  > acl-2.txt

    cat acl-2.txt

#### Authenticate as a service account in Cloud **Shell**

In Cloud Shell, execute the following command to view the current configuration:

    gcloud config list

In Cloud Shell, execute the following command to change the authenticated user to the first service account 

    gcloud auth activate-service-account --key-file credentials.json

To verify the active account, execute the following command:

    gcloud config list

To verify the list of authorized accounts in Cloud Shell, execute the following command:

    gcloud auth list

To verify that the current account (test-service-account) cannot access the cat.jpg file in the first bucket that you created, execute the following command:

    gsutil cp gs://$MY_BUCKET_NAME_1/cat.jpg ./cat-copy.jpg

Verify that the current account (test-service-account) can access the cat.jpg file in the second bucket that you created:

    gsutil cp gs://$MY_BUCKET_NAME_2/cat.jpg ./cat-copy.jpg

To switch to the lab account, execute the following command.
You replace [USERNAME] with the username provided in the Qwiklabs Connection Details pane on the left of the lab instructions page. 

    gcloud config set account [USERNAME]

To verify that you can access the cat.jpg file in the [BUCKET_NAME] bucket (the first bucket that you created), execute the following command.

    gsutil cp gs://$MY_BUCKET_NAME_1/cat.jpg ./copy2-of-cat.jpg

Make the first Cloud Storage bucket readable by everyone, including unauthenticated users.

     gsutil iam ch allUsers:objectViewer gs://$MY_BUCKET_NAME_1