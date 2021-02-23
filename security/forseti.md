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

## enforcer

the enforcer will play in the firewall using json configuration files by allowing or blocking traffic.

### command line tool

this tool has no dependencies, it can be run directely and manually.
however, it seems that the dry-run option is not well supported in beta right now.

https://forsetisecurity.org/docs/latest/use/cli/enforcer.html

### real-time enforcer

this one will use open policy agent to leverage the real-time aspect of the enforcement.
there is quiet a few dependecies and complexity to put the real-time enforcer running.
a side note, the enforcer is very destructive in its nature by destructively enforcing the firewall.

https://www.openpolicyagent.org/docs/latest/
https://forsetisecurity.org/docs/v2.23/configure/real-time-enforcer/index.html
