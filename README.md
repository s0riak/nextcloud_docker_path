# About
This a minimal example on howto serve nextcloud with a host and a path (i.e. /nextcloud).

To achieve this the setup includes to nginx containers:
1. One provided in the nextcloud boilerplate
https://github.com/nextcloud/docker/tree/master/.examples/docker-compose/insecure/mariadb/fpm/web
handling all nextcloud specifics
2. Second added as a reverse proxy handles the path mapping and which can be extended to handling ssl termination
and service other apps with other paths

# Configuration
To define the path under which nextcloud should be served
1. Adapt the location in ``entry_nginx/nginx.conf`` from ``/nextcloud/`` to the desired path
2. Adapt ``compose.yml``
    1. Change the ``OVERWRITEWEBROOT`` environment variable from ``/nextcloud`` to the desired path
(without trailing slash)
    2. Change the ``NEXTCLOUD_TRUSTED_DOMAINS`` to the hostname of your machine, if you are not calling it from the same
   machine

# Usage
Just run
```bash
sudo docker compose up
```
to start the setup.
The setup is now running in

## Purging the environment
Run
```bash
sudo docker compose rm --stop --volumes && sudo docker volume rm nextcloud_docker_path_mariadb
 nextcloud_docker_path_nextcloud &&  sudo rm -rf nextcloud
```
to purge environment and start over with a new path. Deletion of the mariadb volume is required if something went wrong during initial creation (see. hint 1). Deletion of the nextcloud volume is required in case you run the setup with more than 1 nextcloud major version published since the last run.

**Warning: This deletes all content in the nextcloud instance (db and filesystem)!**

# Hints
1. The ```OVERWRITEWEBROOT``` variable in ```compose.yml``` is only respected on initial container
creation, which seems to be undocumented. See
https://stackoverflow.com/questions/52747208/nextcloud-trusted-domain-with-auto-configuration-via-environment-variables
See the purging of the setup in Usage.
3. The ```NEXTCLOUD_TRUSTED_DOMAINS``` needs to be adapted to match the hostname of the ```entry_nginx```
3. Nextcloud requires the headers which are passed by ```entry_nginx/nginx.conf```
4. After install in some cases an 500 error with ```Table ‘nextcloud.oc_appconfig’ doesn’t exist``` in the logs
```sudo docker exec -i nextcloud_path_nextcloud less /var/www/html/data/nextcloud.log``` occurred. Purging the
environment seems to help, which is kind of disturbing.



      