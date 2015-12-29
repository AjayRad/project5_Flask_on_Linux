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
