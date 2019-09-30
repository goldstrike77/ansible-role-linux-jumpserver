![](https://img.shields.io/badge/Ansible-jumpserver-green.svg?logo=angular&style=for-the-badge)

>__Please note that the original design goal of this role was more concerned with the initial installation and bootstrapping environment, which currently does not involve performing continuous maintenance, and therefore are only suitable for testing and development purposes,  should not be used in production environments.__

>__请注意，此角色的最初设计目标更关注初始安装和引导环境，目前不涉及执行连续维护，因此仅适用于测试和开发目的，不应在生产环境中使用。__
___

<p><img src="https://raw.githubusercontent.com/goldstrike77/goldstrike77.github.io/master/img/logo/logo_jumpserver.png" align="right" /></p>

__Table of Contents__

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [JumpServer Versions](#JumpServer-versions)
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
This Ansible role installs JumpServer on linux operating system, including establishing a filesystem structure and server configuration with some common operational features.

## Requirements
### Operating systems
This role will work on the following operating systems:

  * CentOS 7

### JumpServer versions

The following list of supported the JumpServer releases:

* JumpServer 1.4.1x

## Role variables
### Main parameters #
There are some variables in defaults/main.yml which can (Or needs to) be overridden:

##### General parameters
* `jumpserver_ass_linux`: A boolean value, whether to enable Linux assets.
* `jumpserver_ass_windows`: A boolean value, whether to enable Windows assets.
* `jumpserver_version`: Specify the JumpServer version.
* `jumpserver_env`: Path to Python virtualenv directory to install into. 
* `jumpserver_path`: This directory is used to store JumpServer server state.
* `jumpserver_admin_password`: Password for admin user.
* `jumpserver_secret_key`: Secret key.
* `jumpserver_bootstrap_token`: Bootstrap token.

##### Role dependencies
* `jumpserver_redis_dept`: A boolean value, whether installs Redis.
* `jumpserver_mysql_dept`: A boolean value, whether installs MySQL.
* `jumpserver_ngx_dept`: A boolean value, whether proxy web interface and API traffic using NGinx.
* `jumpserver_tomcat_dept`: A boolean value, whether installs Apache Tomcat.

##### Listen port
* `jumpserver_port_arg.server`: JumpServer WEB / API network communication ports.
* `jumpserver_port_arg.coco_httpd`: Coco HTTPD network communication ports.
* `jumpserver_port_arg.coco_sshd`: Coco SSHD network communication ports.

##### Redis parameters
* `jumpserver_redis_path`: Specify the Redis data directory.
* `jumpserver_redis_requirepass`: Authorization clients password.
* `jumpserver_redis_maxmemory`: A memory usage limit to the specified amount in MB.
* `jumpserver_redis_hosts`: Redis hosts address.
* `jumpserver_redis_port`: Redis listen port.

##### MySQL parameters
* `jumpserver_mysql_sa_pass`: MySQL root account password.
* `jumpserver_mysql_user_pass`: MySQL user account password.
* `jumpserver_mysql_path`: Specify the MySQL data directory.
* `jumpserver_mysql_port_mysqld`: MySQL instance listen port.
* `jumpserver_mysql_mailto`: MySQL report mail recipient.
* `jumpserver_mysql_backupset_encryptkey`: BackupSet encryption key, Generate by [openssl rand -base64 24].
* `jumpserver_mysql_backupset_arg`: MySQL backup parameters.
* `jumpserver_mysql_bu_dbs_arg`: JumpServer database variables.

##### NGinx parameters
* `jumpserver_ngx_site_path`: Specify the NGinx site directory.
* `jumpserver_ngx_logs_path`: Specify the NGinx logs directory.
* `jumpserver_ngx_block_agents`: Enables or disables block unsafe User Agents.
* `jumpserver_ngx_block_string`: Enables or disables block includes Exploits / File injections / Spam / SQL injections.
* `jumpserver_ngx_compress`: Enables or disables compression.
* `jumpserver_ngx_domain`: Defines domain name.
* `jumpserver_ngx_port_http`: NGinx HTTP listen port.
* `jumpserver_ngx_port_https`: NGinx HTTPs listen port.
* `jumpserver_ngx_ssl_protocols`: intermediate or modern, defines SSL protocol profile.
* `jumpserver_ngx_version`: extras or standard
* `jumpserver_ngx_client_max_body_size`: The maximum allowed size of the client request body.

##### Tomcat parameters
* `jumpserver_tomcat_port_http`: Tomcat HTTP connectors
* `jumpserver_tomcat_path`: Specify the Tomcat working directory
* `jumpserver_tomcat_disable_ipv6`: Deny IPv6 socket connect If IPv6 is available on the operating system
* `jumpserver_tomcat_jvm_metaspace`: Size of the metaspace in MB.
* `jumpserver_tomcat_jvm_xmn`: Size of the heap for the young generation in MB.
* `jumpserver_tomcat_jvm_xmx`: Size of the heap in MB.
* `jumpserver_tomcat_custom_arg`: Tomcat Custom Variables.

### Other parameters
There are some variables in vars/main.yml:

## Dependencies
- Ansible versions >= 2.8 are supported.
- [NGinx](https://github.com/goldstrike77/ansible-role-linux-nginx.git)
- [Tomcat](https://github.com/goldstrike77/ansible-role-linux-tomcat.git) 
- [MySQL](https://github.com/goldstrike77/ansible-role-linux-mysql.git)
- [Redis](https://github.com/goldstrike77/ansible-role-linux-redis.git)

## Example

### Hosts inventory file
See tests/inventory for an example.

### Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: all
      roles:
         - role: ansible-role-linux-jumpserver

### Combination of group vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: group_vars/all or host_vars/`group_name`

    jumpserver_version: '1.4.10'
    jumpserver_ass_linux: true
    jumpserver_ass_windows: true
    jumpserver_env: '/opt/py3'
    jumpserver_path: '/data'
    jumpserver_admin_password: 'changeme'
    jumpserver_secret_key: 'zTl19IAMF1Zd9f9pbm4F6QammL4kXOI9MsDM6xfhwGgl9io2gU'
    jumpserver_bootstrap_token: 'fq25Q00WJMdmDoZP'
    jumpserver_redis_dept: true
    jumpserver_mysql_dept: true
    jumpserver_ngx_dept: true
    jumpserver_tomcat_dept: true
    jumpserver_port_arg:
      server: '18080'
      coco_httpd: '5000'
      coco_sshd: '2222'
    jumpserver_redis_path: '{{ jumpserver_path }}'
    jumpserver_redis_requirepass: 'changeme'
    jumpserver_redis_maxmemory: '128'
    jumpserver_redis_hosts: '127.0.0.1'
    jumpserver_redis_port: '6379'
    jumpserver_mysql_sa_pass: 'changeme'
    jumpserver_mysql_user_pass: 'changeme'
    jumpserver_mysql_path: '{{ jumpserver_path }}'
    jumpserver_mysql_port_mysqld: '3306'
    jumpserver_mysql_mailto: 'somebody@example.com'
    jumpserver_mysql_backupset_encryptkey: 'eMEVKiBd0HwByKfqv52EE8rmMMa2zg1i'
    jumpserver_mysql_backupset_arg:
      life: '604800'
      keep: '2'
      encryptkey: '{{ jumpserver_mysql_backupset_encryptkey }}'
    jumpserver_mysql_bu_dbs_arg:
      - dbs: 'jumpserver'
        user: 'jumpserver'
        host: '127.0.0.1'
        pass: '{{ jumpserver_mysql_user_pass }}'
        priv: 'ALL'
    jumpserver_ngx_site_path: '/{{ jumpserver_path }}/nginx_site'
    jumpserver_ngx_logs_path: '/{{ jumpserver_path }}/nginx_logs'
    jumpserver_ngx_domain: 'jump.example.com'
    jumpserver_ngx_block_agents: false
    jumpserver_ngx_block_string: false
    jumpserver_ngx_compress: false
    jumpserver_ngx_domain: 'jump.example.com'
    jumpserver_ngx_port_http: '80'
    jumpserver_ngx_port_https: '443'
    jumpserver_ngx_ssl_protocols: 'modern'
    jumpserver_ngx_version: 'extras'
    jumpserver_ngx_client_max_body_size: '50m'
    jumpserver_tomcat_port_http: '8080'
    jumpserver_tomcat_path: '{{ jumpserver_path }}'
    jumpserver_tomcat_disable_ipv6: true
    jumpserver_tomcat_jvm_metaspace: '256'
    jumpserver_tomcat_jvm_xmn: '400'
    jumpserver_tomcat_jvm_xmx: '2048'
    jumpserver_tomcat_custom_arg:
      JUMPSERVER_SERVER: 'http://127.0.0.1:{{ jumpserver_port_arg.server }}'
      JUMPSERVER_KEY_DIR: '/config/guacamole/keys'
      GUACAMOLE_HOME: '/config/guacamole'
      BOOTSTRAP_TOKEN: '{{ jumpserver_bootstrap_token }}'

## License
![](https://img.shields.io/badge/MIT-purple.svg?style=for-the-badge)

## Author Information
Please send your suggestions to make this role better.

## Contributors
Special thanks to the [Connext Information Technology](http://www.connext.com.cn) for their contributions to this role.
