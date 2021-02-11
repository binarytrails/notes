# logstash

## stdin to stdout

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

## stdin to http

    $ logstash --debug -e \
        'input { stdin { } } output { http { "http_method" => "get" url => "https://google.com" } }'
