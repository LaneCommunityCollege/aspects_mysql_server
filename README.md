# aspects_mysql_server

An Ansible role to install and managed mysql server databases and users.

## Requirements

Set ```hash_behaviour=merge``` in your ansible.cfg file.

## Role Variables

### `aspects_mysql_server_enabled`
Boolean. Whether the tasks in this role should be run or not.

Default: `False`

Set to `True` to run this role.
### `aspects_mysql_server_init`
Boolean. Whether the the initial server initialization tasks should be run.

These tasks create the aspectsadmin user, secure the default root users, and run tasks required to set up a new server on RedHat based systems.

You should only have to set this to `True` once per mysql server.

Default: `False`

Set to `True` to run the tasks.

### `aspects_mysql_server_root_users`
A dictionary of root users created by many OS's on installation of MySQL. See [defaults/main.yml](defaults/main.yml) for the value. You may add or remove as needed.

### `aspects_mysql_server_root_password`
The password for all root users. It has no default.

Be sure to set it in a secure file. Consider using ansible-vault, or a similar tool.
### `aspects_mysql_server_aspectsadmin_password`
The password for aspects admin user. It has no default.

Be sure to set it in a secure file. Consider using ansible-vault, or a similar tool.
### `aspects_mysql_server_aspectsadmin_password_change`
Boolean. Should the tasks that change the password for the aspectsadmin user be run, or not.

Default: `False`

### `aspects_mysql_server_root_password_change`
Boolean. Should the tasks that change the password for the root users be run, or not.

Default: `False`

### `aspects_mysql_server_restart_on_config_change`
Boolean. Should Ansible trigger a restart when configuration files change, or not.

MySQL server restarts should only occur when absolutely needed. Thus, this role will not run any handlers unless this variable is changed.

Default: `False`
### `aspects_mysql_server_cnf_path`
The path to the MySQL configuration file loaded when MySQL starts.

### `aspects_mysql_server_configuration`
A dictionary/hash of configuration groups and variables.

The following values were taken from the configuration present after installing mysql server for the first time on vagrant instances of these boxes:
* bento/ubuntu-16.04 (virtualbox, 201803.24.0)
* ubuntu/trusty64    (virtualbox, 20180604.0.0)
* centos/7           (virtualbox, 1804.02)
* debian/stretch64   (virtualbox, 9.4.0)

When all values were the same, the default was left as is.

When there was a difference, the differing value was set for the appropriate OS.

Use the following pattern when adding new variables:

```yaml
aspects_mysql_server_configuration:
  <group>:
    <variable key>:
      enabled: <True|False>
      value:
        default: "<value to use if no OS specific value is set."
        <ansible_distribution>:
          <ansible_distribution_release or ansible_distribution_major_version>: "<value specific to the desired OS>"
```
If you wish to set a value only for a specific OS, you may simply specify that OS and not set a default. 

#### Variables with no value
Some MySQL variables have no values. For example `skip_log_error`. If you need to set one of these variables, simply set the value to `aspects_novalue`. The template will then only add the key.

### `aspects_packages_packages`
A dictionary/hash of packages to install. See the aspects_packages readme for details.
\

## Dependencies
### aspects_packages
`aspects_packages` is used to install and remove OS packages.

### aspects_iptables

`aspects_iptables` is a dependency only when you are using it with `aspects_iptables_enabled` set to True.

## Example Playbook
```yaml
- hosts:
  - aspectscentos7
  - aspectstrusty
  - aspectsxenial
  - aspectsstretch
  vars:
    aspects_packages_enabled: True
    aspects_mysql_server_enabled: True
    aspects_mysql_server_init: False
    aspects_mysql_server_root_password: "password"
    aspects_mysql_server_aspectsadmin_password: "password"
    aspects_mysql_server_aspectsadmin_password_change: False
    aspects_mysql_server_configuration:
      mysqld_safe:
        syslog:
          enabled: True
          value:
            Debian:
              stretch: "aspects_novalue"
            Ubuntu:
              trusty: "aspects_novalue"
              xenial: "aspects_novalue"
      mysqld:
        bind-address:
          enabled: True
          value:
            default: "123.123.123.123"
        key_buffer_size:
          enabled: True
          value:
            default: "32M"
    aspects_mysql_server_databases:
      aspects_test:
        state: "absent"
        name: "aspects_test"
        collation: "utf8mb4_unicode_ci"
        encoding: "utf8mb4"
      aspects_test2:
        state: "present"
        name: "aspects_test2"
        collation: "utf8mb4_unicode_ci"
        encoding: "utf8mb4"
    aspects_mysql_server_users:
      aspectstestuser:
        state: "present"
        name: "aspectstestuser"
        password: "password"
        host: "localhost"
        dbhost: "localhost"
        priv: "*.*:ALL"
  roles:
  - aspects_mysql_server
```

# License

MIT
