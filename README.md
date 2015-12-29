# project5_Flask_on_Linux
Ubuntu server : Host Name  : ec2-52-25-151-135.us-west-2.compute.amazonaws.com 

1. Login to server as root 
  * chmod 600 ~/.ssh/udacity_key.rsa
  * ssh -i ~/.ssh/udacity_key.rsa root@52.25.151.135
2. Create new user "grader":
  * Reference: https://www.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps
  * $ adduser grader
  * Give new user the permission to sudo 
        $ visudo
        Add the following line below root ALL...: NEWUSER ALL=(ALL:ALL) ALL
3. Update and upgrade all currently installed packages
  * $ sudo apt-get update
  * $ sudo apt-get upgrade
4. Change the SSH port from 22 to 2200 and configure SSH access 
  * Reference: https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
  * Change ssh config file:
     * $ vim /etc/ssh/sshd_config
     * Change to Port 2200. 
     * Change PermitRootLogin from without-password to no. 
     * Append UseDNS no.
     * Append AllowUsers NEWUSER.
     * Restart SSH Service: # service ssh restart 
5. Generate a SSH key pair on the local machine:
    * $ ssh-keygen
    * Copy the public id to the server: $ ssh-copy-id username@remote_host -p**_PORTNUMBER_**
    * Login with the new user:     $ ssh -v grader@PUBLIC-IP-ADDRESS -p2200
    * Open SSHD config:   $ sudo vim /etc/ssh/sshd_config
    * Get rid of the warning message sudo: unable to resolve host ... when sudo is executed:Source: Ask Ubuntu
    * Open $ vim /etc/hostname; Copy the hostname.; Append the hostname to the first line: $ sudo sudonano /etc/hosts
6. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123) Ref : https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-14-04
7. Configure the local timezone to UTC  
    * Ref: Ubuntu documentation: https://help.ubuntu.com/community/UbuntuTime#Using_the_Command_Line_.28terminal.29
8. Install and configure Apache to serve a Python mod_wsgi application
    * Install Apache web server:    $ sudo apt-get install apache2
    * Open a browser and open the public ip address, should say 'It works!' on the top of the page.
    * Install mod_wsgi for serving Python apps from Apache and the helper package python-setuptools:     $ sudo apt-get install python-setuptools libapache2-mod-wsgi
    * Restart the Apache server for mod_wsgi to load:
       * sudo service apache2 restart
       * /etc/init.d/apache2 reload 
9. Install and configure git
10. Install & Configure Postgres db  ; add user catalog 
    * ref : http://killtheyak.com/use-postgresql-with-django-flask/
    * Install PostgreSQL:  $ sudo apt-get install postgresql postgresql-contrib
    * Check that no remote connections are allowed (default): $ sudo vim /etc/postgresql/9.3/main/pg_hba.conf
    * Create needed linux user for psql:   $ sudo adduser catalog (choose a password)\
    * Change to default user postgres:     $ sudo su - postgres
    * Connect to the system:     $ psql\
    * Add postgre user with password:
      * Create user with LOGIN role and set a password: 
      * # CREATE USER catalog WITH PASSWORD 'PW-FOR-DB'; (# stands for the command prompt in psql)
      * Allow the user to create database tables: # ALTER USER catalog_pg CREATEDB;   
      * # CREATE DATABASE catalog_pg WITH OWNER catalog;   
      * Connect to the database catalog # \c catalog_pg
      * Revoke all rights: # REVOKE ALL ON SCHEMA public FROM public;
      * Grant only access to the catalog role: # GRANT ALL ON SCHEMA public TO catalog_pg; 
      * list all Databases - /l
      * list all tables in current db - /dt
      * Exit out of PostgreSQl and the postgres user:     # \q, then $ exit
      * Update config file in application to connect to right DB: 
      * $ sudo vim config.py 
      * Change the line starting with "engine" to (fill in a password):         SQLALCHEMY_DATABASE_URI ='postgresql://catalog:PW-FOR-DB@localhost/catalog_pg'
    
