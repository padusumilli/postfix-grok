# postfix-grok

Patterns for parsing postfix maillog using grok. The patterns are adopted from other sources like
- http://blog.tinle.org/?p=459
- https://github.com/whyscream/postfix-grok-patterns

Below is the sample logstash config to test the above patterns, the pattern file is expected to be
in `/etc/logstash/config/patterns` directory

```
input {
  file {
    path => "/var/log/maillog*"
    start_position => "beginning"
  }
}
filter {
  grok {
    patterns_dir => ["/etc/logstash/config/patterns"]
    match => { "message" => "%{PF}" }
  }
  date {
    match => [ "timestamp", "MMM dd HH:mm:ss", "MMM d HH:mm:ss" ]
  }  
}
output {
  stdout { codec => rubydebug }
}
```
