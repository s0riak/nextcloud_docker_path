# About
This a minimal example on howto serve nextcloud with a host and a path.

To achieve this the setup includes to nginx containers:
1. One provided in the nextcloud boilerplate
https://github.com/nextcloud/docker/tree/master/.examples/docker-compose/insecure/mariadb/fpm/web
handling all nextcloud specifics
2. Second added as a reverse proxy handles the path mapping and which can be extended to handling ssl termination
and service other apps with other paths

# Usage
Just run ```sudo docker compose up``` to start the setup.

Run
```bash
sudo docker compose rm --stop --volumes && sudo docker volume rm nextcloud_docker_subpath_mariadb &&
 sudo rm -rf nextcloud
```
to purge environment and start over with a new path.

# Hints
1. The ```OVERWRITEWEBROOT``` variable in ```compose.yml``` is only respected on initial container
creation, which seems to be undocumented. See
https://stackoverflow.com/questions/52747208/nextcloud-trusted-domain-with-auto-configuration-via-environment-variables
See the purging of the setup in Usage.
3. The ```NEXTCLOUD_TRUSTED_DOMAINS``` needs to be adapted to match the hostname of the ```entry_nginx```
3. Nextcloud requires the headers which are passed by ```entry_nginx/nginx.conf```
4. After install in some cases an 500 error with ```Table ‘nextcloud.oc_appconfig’ doesn’t exist``` in the logs
```sudo docker exec -i nextcloud_subpath_nextcloud less /var/www/html/data/nextcloud.log``` occurred. Purging the
environment seems to help, which is kind of disturbing.



      