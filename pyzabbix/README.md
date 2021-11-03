pyzabbix
========

**pyzabbix** is a python module for working with the [Zabbix API](https://www.zabbix.com/documentation/2.2/manual/api).

The Zabbix API is a web based API and is shipped as part of the web frontend. It uses the JSON-RPC 2.0 protocol which means two things:

*   The API consists of a set of separate methods,like "user.login","item.create",etc.
*   Requests and responses between the clients and the API are encoded using the JSON format.



There are some examples using this method：
```
[root@test Zabbix-PyZabbix]# python zabbix_host_delete.py -h
Usage: zabbix_host_delete.py [options]

Options:
  -h, --help            show this help message and exit
  -s SERVER, --server=SERVER
                        (REQUIRED)Zabbix Server URL.
  -u USERNAME, --username=USERNAME
                        (REQUIRED)Username (Will prompt if not given).
  -p PASSWORD, --password=PASSWORD
                        (REQUIRED)Password (Will prompt if not given).
  -H HOSTNAME, --hostname=HOSTNAME
                        (REQUIRED)hostname for hosts.
  -f FILE, --file=FILE  Load values from input file. Specify - for standard
                        input Each line of file contains whitespace delimited:
                        <hostname>
```



python zabbix_host_add.py -s http://zabbix1-x5.prod.lab.x5.ru -u api -p PASSWORD -H router01.lab.x5.ru -g LENVENDO -t Template OS Linux -i 172.20.32.2 -n router01.lab.x5.ru

python zabbix_host_add.py -s SERVER -u USERNAME -p PASSWORD -f FILE

<hostname>4space<ip>4space<groups>4space<templates>

```
python zabbix_host_add.py -h

Usage: zabbix_host_add.py [options]

Options:
  -h, --help            show this help message and exit
  -s SERVER, --server=SERVER
                        (REQUIRED)Zabbix Server URL.
  -u USERNAME, --username=USERNAME
                        (REQUIRED)Username (Will prompt if not given).
  -p PASSWORD, --password=PASSWORD
                        (REQUIRED)Password (Will prompt if not given).
  -H HOSTNAME, --hostname=HOSTNAME
                        (REQUIRED)hostname for hosts.
  -g GROUPS, --groups=GROUPS
                        Host groups to add the host to.If you want to use
                        multiple groups,separate them with a ','.
  -t TEMPLATES, --templates=TEMPLATES
                        Templates to be linked to the host.If you want to use
                        multiple templates, separate them with a ','.
  -i IP, --ip=IP        (REQUIRED)ip address for hosts.
  -n NAME, --name=NAME  Visible name of the host.
  --proxy=PROXY         name of the proxy that is used to monitor the host.
  --status=STATUS       Status and function of the host.  Possible values are:
                        0 - (default) monitored host; 1 - unmonitored host.
  -f FILE, --file=FILE  Load values from input file. Specify - for standard
                        input Each line of file contains whitespace delimited:
                        <hostname>4space<ip>4space<groups>4space<templates>



```