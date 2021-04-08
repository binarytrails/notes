# splunk phantom

SOAR (Security Operations Automation & Response).

## apps

### assets

An App has Assets, define those for service account specific access:

1. Select the App's asset > edit
2. Asset Settings > Content of Service Account JSON file (creds.json)

Don't define variables following the app docs often it will not work. Try previous first.

### logs

    /var/log/phantom/spawn.log          # ingestion
    /var/log/phantom/wsgi.log           # sockets

### development

There are various projects mocking the Phantom env otherwise, Phantom has a community ```.ova``` with a free lisence for one user.

In the end, to be able to install the app you will need to compile it on a Phantom instance since their compiling script is only given in ```pyc```.

https://docs.splunk.com/Documentation/Phantom/4.10.2/DevelopApps/Tutorial

## playbooks

### flow

- Run for a specific case from the debug panel.

- If you edit the Playbook code with UI Editor, Visual Editor won't be available anymore until all changes revert.

### star wars: data vs data*

    data* = one query per node run = multiplexing

    1.1 Check for None with Filter > artifact*... != None
    1.2 Take the filtered value then in {} Format add as Tempalte paramters

    data = list of possible choices all at once

### format

- Json format uses double quotes ```{{"key":"value"}}``` to say it is a json object.
- Use %% something %% to multiplex

## artifacts

In the context of playbooks, they can be called in a multiplexing way.
They referenced in {} Format as Template Parameters.

## apps vs playbooks

You can chain playbooks by calling one another.
