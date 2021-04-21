# splunk

## regex

    | rex field=data.payload.raw "^<(?<id>.*)>.*digitkey=(?<digitkey>\d+)\s.*wordkey=\"(?<wordkey>\w+)\"\s(?<tail>.*)$"

    # matching n occurences
    "^[a-z]{0,10}$"                     # a-z from 0 to 10
                                        # {3} Exactly 3 occurrences;
                                        # {6,} At least 6 occurrences;
                                        # {2,5} 2 to 5 occurrences.

## filtering

    # avoid nulls
    index="sample" | rex ... | where isnotnull(field1)
    index="sample" "data.payload.raw"="*" | rex ...
    
    # keep only uniques
    | dedup field1

## formatting

    # tables
    | table field1, field2 | where isnotnull(field1)

    # grouping
    | stats count by myfield
    | stats values(user) by source, dest

## searches

    # find who searched what
    index=_audit action=search search=* NOT "typeahead" NOT metadata NOT " | history" NOT "AUTOSUMMARY" | table _time, user, search

    # find the user that searched something
    index=_audit action=search search=* user=${user_of_interest} NOT "typeahead" NOT metadata NOT " | history" NOT "AUTOSUMMARY" | table _time, search

    # find who created a user
    index=_audit action=edit_user operation=create
        | rename object as user
        | eval timestamp=strptime(timestamp, "%m-%d-%Y %H:%M:%S.%3N")
        | convert timeformat="%d/%b/%Y" ctime(timestamp)
        | table user timestamp

## ui features

    # there is a build-in view for these stats but less friendly for a report
    /app/splunk_instance_monitoring/search_usage_statistics

    # find users and permissions
    /manager/launcher/authentication/users
