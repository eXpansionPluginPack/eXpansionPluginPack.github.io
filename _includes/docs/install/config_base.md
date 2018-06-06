eXpansion2 is a symfony application before anything else, there are therefore a lot of symfony related configurations. 
We will ignore all this configuration and only bother about what is necessary by eXpansion.

Configuration can be found in `app/config` directory. Configuration that interests us are in the fallowing files : 
* `parameters.yml` 
* `expansion.yml`
* `bundles.yml`

If you have not done so already rename the : 
* `parameters.yml.dist` to `parameters.yml`. 
* `expansion.yml.dist` to `expansion.yml`.
* `bundles.yml.dist` to `bundles.yml`.

The following part of the `parameters.yml` file interests us at first: 

```yaml
parameters:
    database_driver: mysql
    database_host: mysql
    database_port: ~
    database_name: expansion
    database_user: root
    database_password: ~

    dedicated_host: dedicated
    dedicated_port: 5000
    dedicated_timeout: 5
    dedicated_user: SuperAdmin
    dedicated_password: SuperAdmin
    dedicated_connection_type: local
```

### Database configurations


* **database_driver** : Database driver to use. 
eXpansion was tested on mysql but should work with other databases as well (mysql/sqlite/pgsql/oracle)

* **database_host** : Host or Ip to connect to the database.
If you are using wamp/xamp it's probably `localhost` or `127.0.0.1`

* **database_port** : if at null(`~`) default will be used

* **database_name** : Name of the database to use. eXpansion should have it's own database and not share it!

* **database_user** : User to connect to the database, this user should be able to create tables as eXpansion intalls
it's own schema |

* **database_password** : Password to connect to thed database.

### Dedicated server configurations


* **dedicated_host** : Host or ip to connect to the dedicated.
If you are using wamp/xampp it's probably `localhost` or `127.0.0.1`

* **dedicated_port** : The xmlrpc port configured in the dedicated config file

* **dedicated_timeout** : Max timeout time, should not be changed

* **dedicated_password** : Unless specific use case it's needs to be SuperAdmin

* **dedicated_connection_type** : local if the dedicated is on the same machine as eXpansion. remote if not
See section below if you configured it as **remote**.


### Configure a MasterAdmin 

In this file replace `login1` by your own login. You may add as many logins as you wish on multiple lines. The admins configured here are **constant**, they can't be altered ingame. You will be able to add new admins ingame.
Example: 

> /app/config/expansion.yml

```yml
e_xpansion_admin_groups:
    groups:
        master_admin:
            label: Master Admin
            logins:
                - login1
                - login2
            permissions: [] # Master_admin has always all permissions.
        admin:
            label: Admin
            logins: []
            permissions:
              - next
              - restart
```
### Enabling disabling Bundles

You can enable and disable a bundle in the `bundles.yml` file. 
The `bundles.yml.dist` File contains the list of all available bundles.

Lines starting with `#` are commented and therefore those bundles will not be loaded 

Example : 

```yaml
#  - \eXpansion\Bundle\LocalMapRatings\LocalMapRatingsBundle
```
