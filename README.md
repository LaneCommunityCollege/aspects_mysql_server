# aspects_mysql_server

An Ansible role to install and managed mysql server databases and users.

## Note
As of this commit, CentOS 7 is not working. There's a strange "NO MORE HOSTS LEFT" message from Ansible after the package install task is run.

# Requirements

```aspects_mysql_server_enabled``` must be set to ```True``` for the tasks to run.

Set ```hash_behaviour=merge``` in your ansible.cfg file.

# Role Variables

## aspects_mysql_server_restart_on_config_change

Default: False

By default, aspects_mysql_server will not restart mysql when ```/etc/my.cnf``` is modified. You must explicitly set ```aspects_mysql_server_restart_on_config_change``` to True if you want mysql restarted. This ensures that you do not accidentally restart mysql when users are relying on it.

See the ```defaults/main.yml``` and ```vars/*``` files to see examples of more detail configuration.

```aspects_mysql_server_root_login_host``` should be the address part of the user root@<your bind address here>. So make sure that user actually exists.

# Dependencies
## aspects_iptables

```aspects_iptables``` is a dependency only when you are using it with ```aspects_iptables_enabled``` set to True.

# License

MIT
