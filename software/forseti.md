# forseti-security

## resources

first thing first it has to use to appropriate resource depending on your prod or pre-prod env:

    # server-config.yaml
    ...
    invetory:
        root_resource_id: organizations/<ID>
        root_resource_id: projects/<ID>         # or, etc.

run the whole setup again and you should it with:

    forseti explainer list_resource

## inventory

if the resources and everything is setup properly, you should see the invetories:

    forseti inventory list
    forseti inventory get <ID>  # this will have the errors

## logs

* /var/log/forseti.log
* /var/log/syslog

### debug

    forseti server log_level set debug
