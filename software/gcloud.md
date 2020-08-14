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

    $ echo -n "tata" | gcloud secrets create topsecret --data-file=- --replication-policy automatic
    Created version [1] of the secret [pubsub-secret].

    gcloud secrets versions list topsecret
    gcloud secrets versions access 1 --secret topmysecret

    $ gcloud secrets versions add topsecret --data-file=-
    tutu
    ^D
    Created version [2] of the secret [topsecret].

### generate json credentials

    gcloud iam service-accounts keys create creds.json --iam-account [NAME]@[PROJECT_ID].iam.gserviceaccount.com

then, reuse them in a Python session:

    import os
    os.environ["GOOGLE_APPLICATION_CREDENTIALS"]="creds.json"

    from google.cloud import secretmanager

    client = secretmanager.SecretManagerServiceClient()

import issue: https://stackoverflow.com/a/60821519

## functions

### deploy

    gcloud functions deploy NAME \
      --source https://source.developers.google.com/projects/PROJECT_ID/repos/REPOSITORY_ID/moveable-aliases/master/paths/SOURCE

## send data

> To directly invoke the function, you need to send a PubsubMessage, which expects base64-encoded data, as the event data

    DATA=$(printf 'Kaboumin'|base64) && gcloud functions call NAME --data '{"data":"'$DATA'"}'

## logging

Read last logs

    gcloud functions logs read NAME

Find accessed secrets

    gcloud logging read "resource.type="audited_resource" AND
    protoPayload.methodName="google.cloud.secretmanager.v1.SecretManagerService.AccessSecretVersion"" --json

## login methods

* by default, add manually the ssh key that will be pushed into authorized-key

* oslogin: use google account and upload your key

### ssh key

    ~/.ssh/google_compute_engine
