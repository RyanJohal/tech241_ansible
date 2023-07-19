# Setting up ansible
1. Launch 3 identical vms, using ubuntu 18.04
2. Name them individuall, controller, app, db.
3. SSH into them
4. On controller run the following: ```sudo apt update -y``` 
5. ```sudo apt-get install software-properties-common```
6. Install ansible ```sudo apt install ansible```
7. Check version ```sudo ansible --version```
8. ```sudo apt-add-repository ppa:ansible/ansible```
9. 
``` 
sudo apt update -y
sudo apt install ansible -y
sudo ansible --version
```
10. Create .ssh key file by copying private key and pasting into folder
```
cd ~/.ssh
sudo nano tech241.pem
sudo chmod 400 tech241.pem
```
11. Check read permissions have been granted to key file ```ls -l``` 
12. On app and db machines run ```sudo apt update -y
sudo apt upgrade -y```
13. ssh into both machines one at a time from controller ``` sudo ssh -i "tech241.pem" ubuntu@ec2-(publicIp).eu-west-1.compute.amazonaws.com```
14. Select yes in both machines
15. Return to controller
16. ```cd```
17. Go to ansible configuration file ```cd /etc/ansible/```
18. Check existing files ```ls```
19. Allow app and db machines through host file ``` sudo nano hosts```
20. Add following into file ```[web]
web-instance ansible_host=34.240.87.96 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem
[db]
db-instance ansible_host=34.240.87.96 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem```
21. Test connection ```sudo ansible web -m ping```