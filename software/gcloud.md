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

## auth

### http

#### curl

    curl -H "Authorization: Bearer $(gcloud auth print-identity-token)" https://<project>.cloudfunctions.net/<function> \
         -H "Content-Type:application/json" -d '{"key":"value"}'

#### python

##### using gcloud tools

    import subprocess
    token = '{}'.format(subprocess.Popen(args="gcloud auth print-identity-token", stdout=subprocess.PIPE, shell=True).communicate()[0])[2:-3]

    import requests
    r = requests.post(url, headers={"Authorization":"Bearer {}".format(token)})

##### using token generator

1. It's important to understand the ```metadata``` server is internal to Google.
2. Make sure to add the callee Service Account to the caller's permissions

    REGION = 'us-central1'
    PROJECT_ID = [PROJECT ID]
    RECEIVING_FUNCTION = [FUNC NAME]
    function_url = f'https://{REGION}-{PROJECT_ID}.cloudfunctions.net/{RECEIVING_FUNCTION}'
    metadata_server_url = \
        'http://metadata/computeMetadata/v1/instance/service-accounts/default/identity?audience='
    token_full_url = metadata_server_url + function_url
    token_headers = {'Metadata-Flavor': 'Google'}

    token_response = requests.get(token_full_url, headers=token_headers)
    jwt = token_response.text
    function_headers = {'Authorization': f'bearer {jwt}'}
    r = requests.get(function_url, headers=function_headers)

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

## scc (security command center)

### sources

Always filter ```sourceProperties.source:<name>```, in terminal:

    gcloud alpha scc sources describe <org-id> --source-display-name "<name>" --project <project>

