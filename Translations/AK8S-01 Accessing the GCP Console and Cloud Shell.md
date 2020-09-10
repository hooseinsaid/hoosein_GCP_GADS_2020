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

    gcloud beta compute --project=qwiklabs-gcp-04-8d95dde77632 instances create first-vm --zone=us-central1-c --machine-type=e2-micro --subnet=default --network-tier=PREMIUM --maintenance-policy=MIGRATE --service-account=850855358212-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/trace.append --tags=http-server --image=debian-10-buster-v20200902 --image-project=debian-cloud --boot-disk-size=10GB --boot-disk-type=pd-standard --boot-disk-device-name=first-vm --no-shielded-secure-boot --no-shielded-vtpm --no-shielded-integrity-monitoring --reservation-affinity=any

#### Create  firewall-rules

    gcloud compute --project=qwiklabs-gcp-04-8d95dde77632 firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

## create service account

    gcloud iam service-accounts create sa-name \
    --description="sa-description" --display-name="sa-display-name"

## Create a second Cloud Storage bucket and verify it in the GCP Console

create bucket storage
    
    gsutil mb gs://qwiklabs-gcp-00-929cb24cea70