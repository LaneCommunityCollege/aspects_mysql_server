# aspects_mysql_server

Install mysql server. Manage mysql users and databases.

# Requirements

```aspects_mysql_server_enabled``` must be set to ```True``` for the tasks to run.

Set ```hash_behaviour=merge``` in your ansible.cfg file.

# Role Variables

See the ```defaults/main.yml``` and ```vars/*``` files to see examples.

```aspects_mysql_server_root_login_host``` should be the address part of the user root@<your bind address here>. So make sure that user actually exists.

# Dependencies
None

# License

MIT
