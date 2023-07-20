# Installing mongodb to db vm using controller
steps
1. cd /etc/ansible/
2. ```sudo nano mongodb-playbook.yml```
3. Paste following playbook
```
---
# create a playbook to install mongodb in db-machine/instance

# who will be host
- hosts: db

# get logs
  gather_facts: yes

# admin access
  become: true

# provide instructions - task
  tasks:


# install mongodb
  - name: Installing Mongodb
    apt: pkg=mongodb state=present

# ensure the db is running


# check the status if it is running (adhoc)




```