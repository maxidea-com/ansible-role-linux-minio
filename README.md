![](https://img.shields.io/badge/Ansible-minio-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__

___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_minio.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Minio Versions](#Minio-versions)
- [ Role variables](#Role-variables)
  * [Main Configuration](#Main-parameters)
  * [Other Configuration](#Other-parameters)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [Hosts inventory file](#Hosts-inventory-file)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group vars and playbook](#combination-of-group-vars-and-playbook)
- [License](#license)
- [Author Information](#author-information)
- [Contributors](#Contributors)

## Overview
This Ansible role installs  Minio on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### Minio versions

The following list of supported the Minio releases:

* All Release

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `minio_cluster`: The Minio distributed cluster name, Must sets in hostvars.
* `minio_path`: Specify the Minio data directory.
* `minio_tenants`: Specify the name of tenants.

##### Role dependencies
* `minio_ngx_dept`: A boolean value, whether proxy web interface and API traffic using NGinx.

##### Listen port
* `minio_start_port`: The start port of Minio instance.

##### NGinx parameters
* `minio_ngx_site_path`: Specify the NGinx site directory.
* `minio_ngx_logs_path`:  Specify the NGinx logs directory.
* `minio_ngx_block_agents`: Enables or disables block unsafe User Agents.
* `minio_ngx_block_string`: Enables or disables block includes Exploits / File injections / Spam / SQL injections.
* `minio_ngx_compress`: Enables or disables compression.
* `minio_ngx_pagespeed`: Enables or disables pagespeed modules.
* `minio_ngx_port_http`: NGinx HTTP listen port.
* `minio_ngx_port_https`: NGinx HTTPs listen port.
* `minio_ngx_ssl_protocols`: intermediate or modern, defines SSL protocol profile.
* `minio_ngx_version`: extras or standard
* `minio_ngx_client_max_body_size`: The maximum allowed size of the client request body.

##### Server System Variables
* `minio_arg.cache`: Whether enable disk caching feature.
* `minio_arg.cache_expiry`: List of cache exclusion patterns.
* `minio_arg.cache_exclude`: Cache expiry duration in days.
* `minio_arg.cache_quota`: Maximum permitted usage of the cache in percentage.
* `minio_arg.compress_enabled`: Allows streaming compression to ensure efficient disk space usage.
* `minio_arg.compress_extensions`: Which extensions are included by default for compression.
* `minio_arg.compress_mime_types`: Which mime-types are included by default for compression.
* `minio_arg.drive_sync`: Enable synchronous mode for all data operations.
* `minio_arg.uid`: System user ID for running minio services.
* `minio_arg.ulimit_nofile`: The number of files launched by systemd.
* `minio_arg.ulimit_nproc`: The number of processes launched by systemd.
* `minio_arg.user`:  System user name for running minio services.
* `minio_arg.webui`: Enable or Disable access to web UI.

##### Service Mesh
* `environments`: Define the service environment.
* `tags`: Define the service custom label.
* `exporter_is_install`: Whether to install prometheus exporter.
* `consul_public_register`: false Whether register a exporter service with public consul client.
* `consul_public_exporter_token`: Public Consul client ACL token.
* `consul_public_http_prot`: The consul Hypertext Transfer Protocol.
* `consul_public_clients`: List of public consul clients.
* `consul_public_http_port`: The consul HTTP API port.

### Other parameters
- Ansible versions >= 2.8
- Python >= 2.7.5

## Dependencies
- Ansible versions > 2.8 are supported.
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

    node01 ansible_host='192.168.1.10' minio_cluster='demo'
    node02 ansible_host='192.168.1.11' minio_cluster='demo'
    node03 ansible_host='192.168.1.12' minio_cluster='demo'
    node04 ansible_host='192.168.1.13' minio_cluster='demo'

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-minio
           minio_cluster: demo

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    minio_path: '/data'
    minio_tenants: '2'
    minio_ngx_dept: false
    minio_start_port: '9000'
    minio_ngx_site_path: '{{ minio_path }}/nginx_site'
    minio_ngx_logs_path: '{{ minio_path }}/nginx_logs'
    minio_ngx_block_agents: true
    minio_ngx_block_string: true
    minio_ngx_compress: true
    minio_ngx_pagespeed: true
    minio_ngx_port_http: '80'
    minio_ngx_port_https: '443'
    minio_ngx_ssl_protocols: 'modern'
    minio_ngx_version: 'extras'
    minio_ngx_client_max_body_size: '100m'
    minio_arg:
      cache: false
      cache_expiry: '90'
      cache_exclude: "*.pdf"
      cache_quota: '20'
      compress_enabled: 'true'
      compress_extensions:
        - 'txt'
        - 'log'
        - 'csv'
        - 'json'
      compress_mime_types:
        - 'text/csv'
        - 'text/plain'
        - 'application/json'
      drive_sync: 'on'
      uid: '2021'
      ulimit_nofile: '65535'
      ulimit_nproc: '65535'
      user: 'minio'
      webui: 'on'
    environments: 'Development'
    tags:
      subscription: 'default'
      owner: 'nobody'
      department: 'Infrastructure'
      organization: 'The Company'
      region: 'IDC01'
    consul_public_register: false
    consul_public_exporter_token: '00000000-0000-0000-0000-000000000000'
    consul_public_http_prot: 'https'
    consul_public_http_port: '8500'
    consul_public_clients:
      - '127.0.0.1'

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
