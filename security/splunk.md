# splunk

## searches

    # find who searched what
    index=_audit action=search search=* NOT "typeahead" NOT metadata NOT " | history" NOT "AUTOSUMMARY" | table _time, user, search

    # find the user that searched something
    index=_audit action=search search=* user=${user_of_interest} NOT "typeahead" NOT metadata NOT " | history" NOT "AUTOSUMMARY" | table _time, search

## ui features

    # there is a build-in view for these stats but less friendly for a report
    /app/splunk_instance_monitoring/search_usage_statistics

    # find users and permissions
    /manager/launcher/authentication/users
