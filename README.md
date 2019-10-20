# mwadman.librenms

An Ansible role that installs [LibreNMS](https://www.librenms.org/) on Ubuntu 18.04, with a Nginx front end.  

## Requirements

None

## Role Variables

###### Install directory

The directory that the [LibreNMS Github repository](https://github.com/librenms/librenms) will be cloned into.

```yaml
librenms_install_dir: /opt/librenms
```

###### System user

The user that will own the files in the install directory above.  
The 'www-data' (Nginx) user will be added to this group to access these files.

```yaml
librenms_system_user: librenms
```

###### Admin user

The default administrative user credentials.  
You will log in as this user first to create other users.

```yaml
librenms_admin_user: admin
librenms_admin_password: admin
librenms_admin_email: ""
```

###### Update settings

Updates can be disabled by setting `librenms_update` to `false`.  
Unstable code is cloned by default. To change to a stable branch, specify a version tag for `librenms_version` instead.

```yaml
librenms_update: true
librenms_version: master
```

###### Database variables

Database user that LibreNMS will use.  
Set `librenms_database_password` to a secure password, and encrypt this with ansible-vault or other.

```yaml
librenms_database_name: librenms
librenms_database_user: librenms
librenms_database_host: localhost
librenms_database_password:
```

###### Database connection

Only change if you are using a remote database instead of a local one.  
Remember to encrypt `librenms_database_login_password` if this is set.

```yaml
librenms_database_login_host: localhost
librenms_database_login_port: 3306
librenms_database_login_user:
librenms_database_login_password:
```

###### PHP version.

Needs to be checked and incremented with each update.

```yaml
librenms_php_version: 7.2
```

###### Timezone

As per [PHP timezones](https://www.php.net/manual/en/timezones.php).

```yaml
librenms_timezone: "UTC"
```

###### Web UI Socket

IP address and port for the web UI to listen on.

```yaml
librenms_listen_address: 0.0.0.0
librenms_listen_port: 80
```

###### SNMP agent

Configuration of LibreNMS's local agent (for responding to SNMP walks from remote machines).  
Set `librenms_snmpd_community_string` to a secure password, and encrypt this with ansible-vault or other.

```yaml
librenms_snmpd_community_string: public
librenms_snmpd_syslocation: ""  # "Rack, Room, Building, City, Country [GPSX,Y]"
librenms_snmpd_syscontact: ""   # "Your Name <your@email.address>"
```

###### SNMP poller

Sets the SNMP community string to use when polling other devices.  
Remember to encrypt `librenms_snmp_community_string` if this is set.

```yaml
librenms_snmp_community_string: public
```

###### SNMP Trap Receiver

Enables and configures SNMP trap receivership.


If left empty, `librenms_snmp_traps_vendors` will be populated with all vendors that LibreNMS knows the MIBs for.  
To change this behaviour, set this to the list of vendors you have in your environment.


Remember to encrypt `librenms_snmp_community_string` if this is set.

```yaml
librenms_snmp_traps_enabled: true
librenms_snmp_traps_vendors: []
librenms_snmp_traps_community_string: public
```

## Dependencies

None

## Example Playbook

```yaml
---
- hosts: librenms
  become: yes
  roles:
    - mwadman.librenms
```
