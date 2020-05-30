# MariaDB Galera Cluster in Docker Containers

This repository maintains the Dockerfile and scripts to build MariaDB Galera Cluster in Docker Containers.

## What does it do

Dockerfile : 

    1. Build from official docker container
    2. Install mariadb-server mariadb-client mariadb-backup and dependencies
    3. Copy files (conf/, my.cnf, init.sh)
    4. Set /init.sh as entrypoint

init.sh :

    1. Modify mysql config file based on environment variables.
    2. Check service in the cluster to see if this is a new cluster
    3. Install mysql from scratch, if mysql data is not found
    4. Updated safe_to_bootstrap to 1 in /var/lib/mysql/grastate.dat, if mysql data is found and this is a new cluster
    5. Start mysqld with different arguments.

## Docker image
 The docker image can be found at https://hub.docker.com/repository/docker/ustcweizhou/mariadb-cluster

## Example

You can test with command:

    docker-compose -f mariadb-cluster-setup.yaml up -d

If it does not work, please download latest docker-compose and try again.

When all containsers are running, there is a nginx container running with nginx-galera.conf to expose the mariadb cluster.

    # docker-compose -f mariadb-cluster-setup.yaml ps
             Name                   Command             State                       Ports
    ------------------------------------------------------------------------------------------------------
    mariadb-cluster_db01_1    /init.sh               Up (healthy)   3306/tcp, 4444/tcp, 4567/tcp, 4568/tcp
    mariadb-cluster_db02_1    /init.sh               Up             3306/tcp, 4444/tcp, 4567/tcp, 4568/tcp
    mariadb-cluster_db03_1    /init.sh               Up             3306/tcp, 4444/tcp, 4567/tcp, 4568/tcp
    mariadb-cluster_dbvip_1   nginx -g daemon off;   Up             0.0.0.0:13306->3306/tcp, 80/tcp
