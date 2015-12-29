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
