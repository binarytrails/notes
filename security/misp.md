# misp

## docs

https://www.misp-project.org/misp-training/misp-training.pdf

## rest api

    MISP modules - Simple REST API mechanism

    http://127.0.0.1:6666/modules   - introspection interface to get all modules available
                                      returns a JSON with a description of each module
    http://127.0.0.1:6666/query     - interface to query a specic module
                                      to send a JSON to query the module

    MISP autodiscovers the available modules and the MISP site administrator can enable modules as they wish.
    If a conguration is required for a module, MISP adds automatically the option in the server settings.

### curl

    curl -v https://<ip>/servers/getPyMISPVersion.json -H Accept: application/json \
        -H Content-type: application/json -H Authorization: ... -k

### pymisp

Find the misp_key at ```/events/automation```

    # keys.py before running examples
    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    misp_url = 'https://<ip>'
    misp_key = ''
    misp_verifycert = True

