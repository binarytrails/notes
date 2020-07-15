# gcloud

## init account

    gcloud auth login
    
## set project

    gcloud config set project <project>

## fetch instances

    gcloud compute instances list

## connect with identity aware proxy

    gcloud compute ssh <vm> --tunnel-through-iap

## login methods

* by default, add manually the ssh key that will be pushed into authorized-key

* oslogin: use google account and upload your key

### ssh key

    ~/.ssh/google_compute_engine
