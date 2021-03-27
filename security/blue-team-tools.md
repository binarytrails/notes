# blue-team tools

## network analysis

- SecurityOnion

    Linux distro for threat hunting, enterprise security monitoring, and log management

    https://github.com/Security-Onion-Solutions/security-onion

- SELKS

    A Suricata based IDS/IPS distro

    https://github.com/StamusNetworks/SELKS

- Malcom

    Easily deployable network traffic analysis tool suite for full packet capture artifacts (PCAP files) and Zeek logs.

    https://github.com/cisagov/Malcolm

## system

- inotify-tools is a C library and a set of command-line programs providing a simple interface to inotify.

        # monitor all files in folder for changes
        $ inotifywait -m -r --format '%w%f %e' -e modify /tmp/test/
        /tmp/test/toto6 MODIFY

    https://github.com/inotify-tools/inotify-tools

- lsof: list open files

        $ cat > /tmp/LOG &
        cat > /tmp/LOG &
        [1] 18083
        $ lsof -p 18083
        lsof -p 18083
        COMMAND   PID   USER   FD   TYPE DEVICE  SIZE/OFF     NODE NAME
        cat     18083 yamato    1w   REG   0,44         0 54550934 /tmp/LOG

        # monitor inotify files
        $ lsof -n | grep inotify

    https://github.com/lsof-org/lsof

## web

- Domain name permutation engine for detecting homograph phishing attacks, typo squatting, and brand impersonation

    https://github.com/elceef/dnstwist

- Lookyloo: preview with screencapture & detect behavior

        lookyloo --query https://example.com --listing

    https://github.com/Lookyloo/PyLookyloo

- JA3 is a standard for creating SSL client fingerprints in an easy to produce and shareable way.

    https://github.com/salesforce/ja3

- dnstwist: Domain name permutation engine for detecting homograph phishing attacks, typo squatting, and brand impersonation

    Usually thousands of domain permutations are generated - especially for longer input domains. In such cases, it may be practical to display only the ones that are registered. Ensure your DNS server can handle thousands of requests within a short period of time. Otherwise, you can specify an external DNS server with --nameservers argument.

        dnstwist --registered domain.name

    https://github.com/elceef/dnstwist

## email

DMARC is an email authentication protocol. It is designed to give email domain owners the ability to protect their domain from unauthorized use, commonly known as email spoofing. The purpose and primary outcome of implementing DMARC is to protect a domain from being used in business email compromise attacks, phishing emails, email scams and other cyber threat activities.

- checkdmarc: A parser for SPF and DMARC DNS records

    https://github.com/domainaware/checkdmarc

- pyemailprotectionslib: Python library for SPF, DKIM, and DMARC email protections.

        import emailprotectionslib.spf as spf
        import emailprotectionslib.dmarc as dmarc
        spf_record = spf.SpfRecord.from_domain("google.com")
        dmarc_record = dmarc.DmarcRecord.from_domain("google.com")

    https://github.com/lunarca/pyemailprotectionslib

- Implementation details & flow chart diagram (at bottom) of particularities

    https://docs.microsoft.com/en-us/microsoft-365/security/office-365-security/use-dmarc-to-validate-email

## ioc

> Indicator of Compromise

- ioc-fanger: Fang and defang indicators of compromise.

        import ioc_fanger

        ioc_fanger.defang("example.com http://bad.com/phishing.php")  # example[.]com hXXp://bad[.]com/phishing[.]php
        ioc_fanger.fang("example[.]com hXXp://bad[.]com/phishing[.]php")  # example.com http://bad.com/phishing.php

    https://github.com/ioc-fang/ioc-fanger

- ipgrep: Extract, defang, resolve names and IPs from text

    https://github.com/jedisct1/ipgrep

- ioc_finder : Simple, effective, and modular package for parsing observables (indicators of compromise (IOCs), network data, and other, security related information) from text. It uses grammars rather than regexes which makes it more readable, maintainable, and hackable.

        from ioc_finder import find_iocs
        text = "This is just an foobar.com https://example.org/test/bingo.php"
        iocs = find_iocs(text)
        print(iocs['domains'])  # ['foobar.com', 'example.org']
        print(iocs['urls'])  # ['https://example.org/test/bingo.php']

    https://github.com/fhightower/ioc-finder

## incident response

- CISA Hunt and Incident Response Program (CHIRP)

    Tool created to dynamically query Indicators of Compromise (IoCs) on hosts with a single package, outputting data in a JSON format for further analysis in a SIEM or other tool. CHIRP does not modify any system data.

    https://github.com/cisagov/CHIRP

## active contributors

- https://github.com/cisagov
