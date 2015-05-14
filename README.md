logstash
=========

Ansible role to manage [logstash](http://logstash.net/).

Requirements
------------

No external requirements.

Role Variables
--------------

Please refer to the [logstash](http://logstash.net)
[reference documentation](http://www.elastic.co/guide/en/logstash/current/index.html)
on how to configure logstash.  If building patterns for grok, the
[grok debugger](http://grokdebug.herokuapp.com/) may help.

- **logstash_user**: User *logstash* runs as.  Defaults to `logstash`.
- **logstash_version**: Version number of *logstash* to install.
  Defaults to `1.4`.
- **logstash_nofile**: Number of files *logstash* user can open.
  Defaults to `16384`.
- **logstash_path_conf**: The configuration directory for *logstash*.
  Defaults to `/etc/logstash/conf.d`.
- **logstash_path_home**: The path to where *logstash* binaries are
  installed.  Defaults to `/opt/logstash`.
- **logstash_path_logs**: The path to log files.  Defaults to `/var/log/logstash`.
- **logstash_path_data**: The directory to store data.  Defaults to
  `/var/lib/logstash`.
- **logstash_java_opts**: Options to pass to JVM.  Defaults to
  `-Djava.io.tmpdir=${LS_HOME}`.
- **logstash_java_heap_size**: Amount of memory for JVM heap size.
  Defaults to `256m`.
- **logstash_inputs**:  A yaml scalar that defines the inputs for
  *logstash*.  Defaults to:
```
logstash_inputs: |-
    file {
      path => [ "/var/log/messages" ],
      type => "syslog"
    }
```
- **logstash_filters**: A yaml scalar that defines the filters for
  *logstash*.  Defaults to:
```
logstash_filters: |-
    if [type] == "syslog" {
      grok { match => { message => "%SYSLOGBASE" } }
      date { match => [ "timestamp", "MMM dd HH:mm:ss" ] }
    }
```
- **logstash_outputs**: A yaml scalar that defines the outputs for
  *logstash*.  Defaults to:
```
logstash_outputs: |-
    elasticsearch {
      host => "localhost"
    }
```
- **logstash_patterns**:  A list of dictionaries, where the keys are
  *name* and *pattern*.  This defaults to an empty list.  An example
  configuration would be:
```
logstash_patterns:
  - name: custom
    pattern: |
      ASA %ASA
```

Dependencies
------------

Depends on [sfromm.elasticsearch](https://github.com/sfromm/ansible-elasticsearch).

Example Playbook
----------------

A trivial example:

    - hosts: servers
      roles:
         - { role: sfromm.elasticsearch }
         - { role: sfromm.logstash }

License
-------

GPLv2

Author Information
------------------

See https://github.com/sfromm
