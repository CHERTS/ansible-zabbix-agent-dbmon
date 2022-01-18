[![Ansible Galaxy](https://img.shields.io/badge/role-cherts.zabbix__agent__dbmon-blue)](https://galaxy.ansible.com/cherts/zabbix_agent_dbmon/)
[![Ansible Galaxy Quality Score](https://img.shields.io/ansible/quality/57627)](https://galaxy.ansible.com/cherts/zabbix-agent-dbmon/)
[![Ansible Galaxy Downloads](https://img.shields.io/ansible/role/d/57627.svg?color=blue)](https://galaxy.ansible.com/cherts/zabbix-agent-dbmon/)

# Zabbix agent DBMON Ansible role

Table of Contents

- [Overview](#overview)
- [Requirements](#requirements)
  * [Operating systems](#operating-systems)
  * [Local system access](#local-system-access)
  * [Zabbix Versions](#zabbix-versions)
    + [Zabbix 5.0](#zabbix-50)
- [Getting started](#getting-started)
  * [Installation](#installation)
  * [Minimal Configuration](#minimal-configuration)
- [Role Variables](#role-variables)
  * [Main variables](#main-variables)
  * [TLS Specific configuration](#tls-specific-configuration)
  * [Zabbix API variables](#zabbix-api-variables)
  * [Other variables](#other-variables)
- [Dependencies](#dependencies)
- [Example Playbook](#example-playbook)
  * [agent_interfaces](#agent-interfaces)
  * [Other interfaces](#other-interfaces)
  * [Vars in role configuration](#vars-in-role-configuration)
  * [Combination of group_vars and playbook](#combination-of-group-vars-and-playbook)
  * [Example for TLS PSK encrypted agent communication](#example-for-tls-psk-encrypted-agent-communication)
- [Molecule](#molecule)
  * [default](#default)
  * [with-server](#with-server)
  * [before-last-version](#before-last-version)
- [Deploying Userparameters](#deploying-userparameters)
- [License](#license)

---------------------------------------------------------------------------------------------

# Introduction

This is an role for installing and maintaining the zabbix-agent-dbmon. It will install the Zabbix Agent for Database Monitoring (DBMON) on any host with an operating system that is defined [here](#operating-systems).

# Requirements

## Operating systems
This role will work on the following operating systems:

 * Red Hat
 * Ubuntu

So, you'll need one of those operating systems.. :-)
Please sent Pull Requests or suggestions when you want to use this role for other Operating systems.

## Local system access

To successfully complete the install the role requires `python-netaddr` on the controller to be able to manage IP addresses. This requires that the library is available on your local machine (or that `pip` is installed to be able to run). This will likely mean that running the role will require `sudo` access to your local machine and therefore you may need the `-K` flag to be able to enter your local machine password if you are not running under root.

## Zabbix Versions

See the following list of supported Operating systems with the Zabbix releases:

### Zabbix 5.0

  * CentOS 7.x
  * CentOS 8.x
  * RedHat 7.x
  * RedHat 8.x
  * OracleLinux 7.x
  * OracleLinux 8.x
  * Ubuntu 16.04, 18.04, 20.04

# Getting started

## Installation

Installing this role is very simple: `ansible-galaxy install cherts.zabbix_agent_dbmon`

This will install the zabbix_agent_dbmon role into your `roles` directory.

## Minimal Configuration

In order to get the Zabbix Agent running, you'll have to define the following properties before executing the role:

* zabbix_version
* zabbix_agent_server
* zabbix_agent_serveractive (When using active checks)

The `zabbix_version` is optional. The latest available major.minor version of Zabbix will be installed on the host(s). If you want to use an older version, please specify this in the major.minor format. Support only: `zabbix_version: 5.0`.

The `zabbix_agent_server` (and `zabbix_agent_serveractive`) should contain the ip or fqdn of the host running the Zabbix Server.

# Role Variables

## Main variables

There are some variables in de default/main.yml which can (Or needs to) be overridden:

* `zabbix_agent_server`: The ip address for the zabbix-server or zabbix-proxy.

* `zabbix_agent_serveractive`: The ipaddress for the zabbix-server or zabbix-proxy for active checks.

* `zabbix_version`: This is the version of zabbix. Default it is 4.0, but can be overridden to one of the versions mentioned in [Zabbix Versions](#zabbix-versions).

* `zabbix_repo`: Default: _zabbix_
  * _epel_ install agent from EPEL repo
  * _dbs_ (default) install agent from DBService repo
  * _other_ install agent from pre-existing or other repo

* `zabbix_agent_listeninterface`: Interface zabbix-agent listens on. Leave blank for all.

* `zabbix_agent_package`: The name of the zabbix-agent package. Default: `zabbix-agent`. In case for EPEL, it is automatically renamed.

* `zabbix_sender_package`: The name of the zabbix-sender package. Default: `zabbix-sender`. In case for EPEL, it is automatically renamed.

* `zabbix_get_package`: The name of the zabbix-get package. Default: `zabbix-get`. In case for EPEL, it is automatically renamed.

* `zabbix_agent_package_state`: If Zabbix-agent needs to be present or latest.

* `zabbix_agent_interfaces`: A list that configured the interfaces you can use when configuring via API.

* `zabbix_selinux`: Enables an SELinux policy so that the agent will run. Default: False.

* `zabbix_agent_userparameters`: List of userparameter names and scripts (if any). Detailed description is given in the [Deploying Userparameters](#deploying-userparameters) section. Default: `[]` (Empty list).
    * `name`: Userparameter name (should be the same with userparameter template file name)
    * `scripts_dir`: Directory name of the custom scripts needed for userparameters

* `zabbix_agent_allowroot`: Allow the agent to run as 'root'. 0 - do not allow, 1 - allow

* `zabbix_agent_runas_user`: Drop privileges to a specific, existing user on the system. Only has effect if run as 'root' and AllowRoot is disabled.

* `zabbix_agent_become_on_localhost`: Set to `False` if you don't need to elevate privileges on localhost to install packages locally with pip. Default: True

* `zabbix_install_pip_packages`: Set to `False` if you don't want to install the required pip packages. Useful when you control your environment completely. Default: True

## TLS Specific configuration

These variables are specific for Zabbix 3.0 and higher:

* `zabbix_agent_tlsconnect`: How the agent should connect to server or proxy. Used for active checks.

    Possible values:

    * unencrypted
    * psk
    * cert

* `zabbix_agent_tlsaccept`: What incoming connections to accept.

    Possible values:

    * unencrypted
    * psk
    * cert

* `zabbix_agent_tlscafile`: Full pathname of a file containing the top-level CA(s) certificates for peer certificate verification.

* `zabbix_agent_tlscrlfile`: Full pathname of a file containing revoked certificates.

* `zabbix_agent_tlsservercertissuer`: Allowed server certificate issuer.

* `zabbix_agent_tlsservercertsubject`: Allowed server certificate subject.

* `zabbix_agent_tlscertfile`: Full pathname of a file containing the agent certificate or certificate chain.

* `zabbix_agent_tlskeyfile`: Full pathname of a file containing the agent private key.

* `zabbix_agent_tlspskidentity`: Unique, case sensitive string used to identify the pre-shared key.

* `zabbix_agent_tlspskfile`: Full pathname of a file containing the pre-shared key.

* `zabbix_agent_tlspsk_secret`: The pre-shared secret key that should be placed in the file configured with `agent_tlspskfile`.

## Zabbix API variables

These variables needs to be overridden when you want to make use of the zabbix-api for automatically creating and or updating hosts.

Host encryption configuration will be set to match agent configuration.

When `zabbix_api_create_hostgroup` or `zabbix_api_create_hosts` is set to `True`, it will install on the host executing the Ansible playbook the `zabbix-api` python module.

* `zabbix_url`: The url on which the Zabbix webpage is available. Example: http://zabbix.example.com

* `zabbix_api_create_hosts`: When you want to enable the Zabbix API to create/delete the host. This has to be set to `True` if you want to make use of `zabbix_create_host`. Default: `False`

* `zabbix_api_create_hostgroup`: When you want to enable the Zabbix API to create/delete the hostgroups. This has to be set to `True` if you want to make use of `zabbix_create_hostgroup`.Default: `False`

* `zabbix_api_user`: Username of user which has API access.

* `zabbix_api_pass`: Password for the user which has API access.

* `zabbix_create_hostgroup`: present (Default) if the hostgroup needs to be created or absent if you want to delete it. This only works when `zabbix_api_create_hostgroup` is set to `True`.

* `zabbix_host_status`: enabled (Default) when host in monitored, disabled when host is disabled for monitoring.

* `zabbix_create_host`: present (Default) if the host needs to be created or absent is you want to delete it. This only works when `zabbix_api_create_hosts` is set to `True`.

* `zabbix_update_host`: yes (Default) if the host should be updated if already present. This only works when `zabbix_api_create_hosts` is set to `True`.

* `zabbix_useuip`: 1 if connection to zabbix-agent is made via ip, 0 for fqdn.

* `zabbix_host_groups`: An list of hostgroups which this host belongs to.

* `zabbix_link_templates`: An list of templates which needs to be link to this host. The templates should exist.

* `zabbix_macros`: An list with macro_key and macro_value for creating hostmacro's.

* `zabbix_inventory_mode`: Configure Zabbix inventory mode. Needed for building inventory data, manually when configuring a host or automatically by using some automatic population options. This has to be set to `automatic` if you want to make automatically building inventory data.

* `zabbix_visible_hostname` : Configure Zabbix visible name inside Zabbix web UI for the node.

* `zabbix_validate_certs` : yes (Default) if we need to validate tls certificates of the API. Use `no` in case self-signed certificates are used

## Other variables

* `zabbix_agent_firewall_enable`: If IPtables needs to be updated by opening an TCP port for port configured in `zabbix_agent_listenport`.

* `zabbix_agent_firewall_source`: When provided, IPtables will be configuring to only allow traffic from this IP address/range.

* `zabbix_agent_firewalld_enable`: If firewalld needs to be updated by opening an TCP port for port configured in `zabbix_agent_listenport` and `zabbix_agent_jmx_listenport` if defined.

* `zabbix_agent_firewalld_source`: When provided, firewalld will be configuring to only allow traffic for IP configured in `zabbix_agent_server`.

# Dependencies

There are no dependencies on other roles.

# Example Playbook

## agent_interfaces

This will configure the Zabbix Agent DBMON interface on the host.
```
zabbix_agent_interfaces:
  - type: 1
    main: 1
    useip: "{{ zabbix_useuip }}"
    ip: "{{ zabbix_agent_ip }}"
    dns: "{{ ansible_fqdn }}"
    port: "{{ zabbix_agent_listenport }}"
```

## Other interfaces

You can also configure the `zabbix_agent_interfaces` to add/configure snmp, jmx and ipmi interfaces.

You'll have to use one of the following type numbers when configuring it:

| Type Interface  |  Nr   |
|-----------------|-------|
| Zabbix Agent  | 1  |
| snmp | 2  |
| ipmi | 3  |
| jmx | 4  |

Configuring a snmp interface will look like this:

```
zabbix_agent_interfaces:
  - type: 2
    main: 1
    useip: "{{ zabbix_useuip }}"
    ip: "{{ agent_ip }}"
    dns: "{{ ansible_fqdn }}"
    port: "{{ agent_listenport }}"
```

## Vars in role configuration
Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```
    - hosts: all
      roles:
         - role: cherts.zabbix_agent_dbmon
           zabbix_agent_server: 192.168.33.30
           zabbix_agent_serveractive: 192.168.33.30
           zabbix_url: http://zabbix.example.com
           zabbix_api_use: true # use zabbix_api_create_hosts and/or zabbix_api_create_hostgroup from 0.8.0
           zabbix_api_user: Admin
           zabbix_api_pass: zabbix
           zabbix_create_host: present
           zabbix_host_groups:
             - PostgreSQL
           zabbix_link_templates:
             - DBS_Template PostgreSQL for Linux
           zabbix_macros:
             - macro_key: DBS_PGSQL_CONN_STRING
               macro_value: host=localhost port=5432 dbname=postgres user=zabbixmon password=XXXXXXX connect_timeout=10
             - macro_key: DBS_PGSQL_SERVICE_CMD_REGEXP
               macro_value: ^.*-D /var/lib/pgsql/9.6/data.*$
             - macro_key: DBS_PGSQL_SERVICE_NAME
               macro_value: postgres
             - macro_key: DBS_PGSQL_SERVICE_USER
               macro_value: postgres
```

## Combination of group_vars and playbook
You can also use the group_vars or the host_vars files for setting the variables needed for this role. File you should change: `group_vars/all` or `host_vars/<zabbix_server>` (Where <zabbix_server> is the hostname of the machine running Zabbix Server)
```
        zabbix_agent_server: 192.168.33.30
        zabbix_agent_serveractive: 192.168.33.30
        zabbix_url: http://zabbix.example.com
        zabbix_api_use: true # use zabbix_api_create_hosts and/or zabbix_api_create_hostgroup from 0.8.0
        zabbix_api_user: Admin
        zabbix_api_pass: zabbix
        zabbix_create_host: present
        zabbix_host_groups:
          - PostgreSQL
        zabbix_link_templates:
          - DBS_Template PostgreSQL for Linux
        zabbix_macros:
          - macro_key: DBS_PGSQL_CONN_STRING
            macro_value: host=localhost port=5432 dbname=postgres user=zabbixmon password=XXXXXXX connect_timeout=10
```

and in the playbook only specifying:
```
    - hosts: all
      roles:
         - role: cherts.zabbix_agent_dbmon
```

## Example for TLS PSK encrypted agent communication

Variables e.g. in the playbook or in `host_vars/myhost`:
```
    zabbix_agent_tlsaccept: psk
    zabbix_agent_tlsconnect: psk
    zabbix_agent_tlspskidentity: "myhost PSK"
    zabbix_agent_tlspsk_secret: b7e3d380b9d400676d47198ecf3592ccd4795a59668aa2ade29f0003abbbd40d
    zabbix_agent_tlspskfile: /etc/zabbix/zabbix_agent_pskfile.psk
```

# License

BSD

# Author Information

Please send suggestion or pull requests to make this role better. Also let me know if you encounter any issues installing or using this role.

Github: https://github.com/CHERTS/ansible-zabbix-agent-dbmon

mail: sleuthhound [ at ] gmail . com
