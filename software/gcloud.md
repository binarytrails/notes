# gcloud

    # init account
    gcloud auth login

    # set project
    gcloud config set project <project>

    # fetch instances
    gcloud compute instances list

    # read last logs
    gcloud functions logs read NAME
    # --format=json --format=csv\('KEY'\) --format=get\('KEY'\) --format=value\('KEY'\)

    # connect with identity aware proxy
    gcloud compute ssh <vm> --tunnel-through-iap
    ls -l ~/.ssh/google_compute_engine

    # create git repo
    gcloud source repos create <repo> --project=<project>
    gcloud source repos clone <repo> --project=<project>

    # secrets: difference with kms is that it abstracts the encryption keys for you
    $ echo -n "tata" | gcloud secrets create topsecret --data-file=- --replication-policy automatic
    $ gcloud secrets versions list topsecret
    $ gcloud secrets versions access 1 --secret topmysecret
    $ gcloud secrets versions add topsecret --data-file=-
    tutu
    ^D

    # read from logs the accessed secrets
    gcloud logging read "resource.type="audited_resource" \
        AND protoPayload.methodName="google.cloud.secretmanager.v1.SecretManagerService.AccessSecretVersion"" --json

    # generate json credentials
    gcloud iam service-accounts keys create creds.json \
        --iam-account [NAME]@[PROJECT_ID].iam.gserviceaccount.com

    # json creds in python
    import os
    os.environ["GOOGLE_APPLICATION_CREDENTIALS"]="creds.json"
    from google.cloud import secretmanager
    client = secretmanager.SecretManagerServiceClient()

    # function deploy
    gcloud functions deploy NAME \
      --source https://source.developers.google.com/projects/PROJECT_ID/repos/REPOSITORY_ID/moveable-aliases/master/paths/SOURCE

    # send data to function
    DATA=$(printf 'Kaboumin'|base64) && gcloud functions call NAME --data '{"data":"'$DATA'"}'

    # http auth with curl
    curl -H "Authorization: Bearer $(gcloud auth print-identity-token)" \
        https://<project>.cloudfunctions.net/<function> -H "Content-Type:application/json" -d '{"key":"value"}'

    # http auth with python
    import subprocess
    token = '{}'.format(subprocess.Popen(
        args="gcloud auth print-identity-token",
              stdout=subprocess.PIPE, shell=True).communicate()[0])[2:-3]
    import requests
    r = requests.post(url, headers={"Authorization":"Bearer {}".format(token)})

    # http auth with python token generator
    # 1. metadata server is internal to Google.
    # 2. make sure to add the callee service account to the caller's permissions
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

## scc (security command center)

    # scc sources: always filter with sourceProperties.source:<name>
    gcloud alpha scc sources describe <org-id> --source-display-name "<name>" --project <project>

    # filtering
    gcloud alpha scc findings list <ord-id> --project <project-id> \
        --filter="state=\"ACTIVE\" AND sourceProperties.source=\"<NAME>\" AND category=\"<NAME>\"" \
        --format="table(finding.category, finding.eventTime,resource.projectDisplayName)" --limit 20

    # filtering faster method
    gcloud alpha scc findings list <org-id> --project <project-id> \
        --format="table(finding.category, finding.eventTime,resource.projectDisplayName)" \
        --source=<source-id> --limit 20

    # findings
    gcloud alpha scc findings list <org-id> --project <project-id> \
        --filter="state=\"ACTIVE\" AND category=\"<CATEGORY>\""

    # notifications
    gcloud alpha scc notifications list <org-id>

    # marks
    gcloud alpha scc assets update-marks organizations/<org-id>/assets/<asset-id> \
        --security-marks allow_public_ip_address=true --update-mask marks.allow_public_ip_address
        --project=<project-id>
