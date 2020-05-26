# mariadb-galera
This repository contains details about setting up and use master-master clustering i.e. galera clustering using maria db.


==== Follow the below steps to setup a galera cluster for Ubuntu 18.04 ======

Run the below commands sequentially:::
1. Install the sofware properties --
	apt-get install python-software-properties --> Debian and Debian-based Linux distributions
	                                            OR
	sudo apt-get install software-properties-common --> Ubuntu or a distribution derived from Ubuntu

2. sudo apt-key adv --fetch-keys 'https://mariadb.org/mariadb_release_signing_key.asc'

3. sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.rackspace.com/mariadb/repo/10.4/ubuntu bionic main'

Now the keys are imported and the repository is added for MariaDB 10.4 stable version

Installing MariaDB now.

4. sudo apt update
   sudo apt install mariadb-server

5. Add debug conponents from sources list --
	sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.rackspace.com/mariadb/repo/10.4/ubuntu bionic main'

The above can be changed and used according to the OS and MariaDB version used.
Refer https://downloads.mariadb.org/mariadb/repositories/


6. Edit galera configuration and set /etc/mysql/conf.d/galera.cnf (Check galera.cnf in repo)
  vim /etc/mysql/conf.d/galera.cnf

7. Stop all the mysql servers 
	service mysql stop
  
8.  Initiate the galera cluster in the Node 1. You just need to init this once.
	galera_new_cluster
  
9. Check the cluster size
	mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'";

10. Update the galera.cnf for Node 2 and start the mysql service (service mysql start). Check the cluster size again using 
    	mysql -u root -p -e "SHOW STATUS LIKE 'wsrep_cluster_size'";
      
11. Repeat the same for all the nodes.

12. Verifying the cluster working.
	create database GALERA_TEST;
	use GALERA_TEST;
	create a sample table and feed some data
	check if the same is reflected in other nodes and vice versa

13. After the connection is done, update galera.cnf for all the nodes and input the IPs of the rest of the nodes available.
