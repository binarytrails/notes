# gcloud

## init account

    gcloud auth login
    
## set project

    gcloud config set project <project>

## fetch instances

    gcloud compute instances list

## connect with identity aware proxy

    gcloud compute ssh <vm> --tunnel-through-iap

## format

    --format=json
    --format=csv\('KEY'\)
    --format=get\('KEY'\)
    --format=value\('KEY'\)

## secrets

*The difference with kms is that it abstracts the encryption keys for you.*

    gcloud secrets
    gcloud secrets versions list mysecret
    gcloud secrets versions access 1 --secret mysecret

    gcloud secrets versions add mysecret --data-file=-
    tutu
    ^D
    Created version [1] of the secret [mysecret].

## logging

Find accessed secrets

    gcloud logging read "resource.type="audited_resource" AND
    protoPayload.methodName="google.cloud.secretmanager.v1.SecretManagerService.AccessSecretVersion"" --json

## login methods

* by default, add manually the ssh key that will be pushed into authorized-key

* oslogin: use google account and upload your key

### ssh key

    ~/.ssh/google_compute_engine
