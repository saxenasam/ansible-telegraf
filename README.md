## Build status:

This role will install and configure telegraf.

Telegraf is an agent written in Go for collecting metrics from the system it's running on, or from other services, and writing them into InfluxDB.

Design goals are to have a minimal memory footprint with a plugin system so that developers in the community can easily add support for collecting metrics from well known services (like Hadoop, Postgres, or Redis) and third party APIs (like Mailchimp, AWS CloudWatch, or Google Analytics).

(https://github.com/influxdb/telegraf)

## Requirements

### Supported systems

This role supports the following systems:

 * Red Hat

So, you'll need one of those systems.. :-)

## Role Variables

### Ansible role specific variables

Specifying the version to be installed:

* `telegraf_agent_version`: The version of Telegraf to install. Default: `1.13.1`

How `Telegraf` needs to be installed.

* Via a download from the `aws s3 bucket` site ("online");

This can be configured by setting `telegraf_agent_package_method` to one of the appropriate values ( `repo`, `online` or `offline`).

#### Telegraf Package

These properties set in how and what package will be installed.

* `telegraf_agent_package`: The name of the Telegraf package to install. When `telegraf_agent_package_method` is set to `online` or `offline`, it needs to have the full path of the file. Example: `telegraf_agent_package: /tmp/telegraf.rpm`. Default: `telegraf_agent_package: telegraf`.
* `telegraf_agent_package_method`: The installation method to be used. Can choose between: `repo`, `offline` or `online`.
* `telegraf_agent_package_state`: If the package should be `present` or `latest`. When set to `latest`, `telegraf_agent_version` will be ignored. Default: `present`

### Telegraf agent process configuration.

* `telegraf_agent_interval`: The interval configured for sending data to the server. Default: `10`
* `telegraf_agent_debug`: Run Telegraf in debug mode. Default: `False`
* `telegraf_agent_round_interval`: Rounds collection interval to 'interval' Default: True
* `telegraf_agent_flush_interval`: Default data flushing interval for all outputs. Default: 10
* `telegraf_agent_flush_jitter`: Jitter the flush interval by a random amount. Default: 0
* `telegraf_agent_aws_tags`: Configure AWS ec2 tags into Telegraf tags section Default: `False`
* `telegraf_agent_aws_tags_prefix`: Define a prefix for AWS ec2 tags. Default: `""`
* `telegraf_agent_collection_jitter`: Jitter the collection by a random amount. Default: 0 (since v0.13)
* `telegraf_agent_metric_batch_size`: The agent metric batch size. Default: 1000  (since v0.13)
* `telegraf_agent_metric_buffer_limit`: The agent metric buffer limit. Default: 10000  (since v0.13)
* `telegraf_agent_quiet`: Run Telegraf in quiet mode (error messages only). Default: `False` (since v0.13)
* `telegraf_agent_logfile`: The agent logfile name. Default: '' (means to log to stdout) (since v1.1)
* `telegraf_agent_omit_hostname`: Do no set the "host" tag in the agent. Default: `False` (since v1.1)

### Setting tags

You can set tags for the host running telegraf:

	telegraf_global_tags:
	  - tag_name: some_name
	    tag_value: some_value

Specifying an output. The default is set to localhost, you'll have to specify the correct influxdb server:

	telegraf_agent_output:
	  - type: influxdb
	    config:
	      - urls = ["http://localhost:8086"]
	      - database = "telegraf"
        tagpass:
          - cpu = ["cpu0"]

The config will be printed line by line into the configuration, so you could also use:

	config:
		- # Print an documentation line

and it will be printed in the configuration file.

## Dependencies

No dependencies

## Example Playbook

    - hosts: servers
      roles:
         - { role: ansible-telegraf }


