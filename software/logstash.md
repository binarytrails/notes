# logstash

## one liners

### stdin to stdout

    $ logstash --debug -e 'input { stdin { } } output { stdout { } }'
    ...
    The stdin plugin is now waiting for input:
    ...
    toto
    {
              "host" => "buster",
           "message" => "toto",
          "@version" => "1",
        "@timestamp" => 2021-01-22T21:22:13.406Z
    }

### stdout with metadata

    output {
        stdout { codec => rubydebug { metadata => true } }
    }

### stdout depending on metadata

    # meta.conf
    input { stdin { } }
    filter {
        mutate { add_field => { "[@metadata][test]" => "toto" } }
    }
    output {
        if [@metadata][test] == "toto" {
            stdout { codec => rubydebug }
        }
    }
    $ logstash -f meta.conf

### stdin to http

    $ logstash --debug -e \
        'input { stdin { } } output { http { "http_method" => "get" url => "https://google.com" } }'

## configurations

Default location `/etc/logstash/conf.d/`

### file input/output

    input {
        file {
            path => "/var/log/logstash/input/*.log"
            start_position => "beginning"
            ignore_older => 0
            sincedb_path => "/dev/null"
        }
    }
    output {
        file {
            path => "/var/log/logstash/out.log"
    }
    stdout { codec => rubydebug }
}

### stackdriver output

    filter {
        json {
            source => "message"
        }
    }
    output {
        stackdriver_logging {
            project_id => "GCP_PROJECT"
            key_file => "/etc/logstash/conf.d/creds.json"
            log_name => "mylogname"
            severity_field => "severity"
            timestamp_field => "@timestamp"
            default_severity => "default"
            strip_at_fields => false
            strip_fields => []
        }
    }

