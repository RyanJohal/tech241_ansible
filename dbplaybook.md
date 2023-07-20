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

# enable
  - name: Enable mongod service
    service:
      name: mongodb
      enabled: yes

# check the status if it is running (adhoc)


# add steps to make required changes to mongod.conf to change ip
  - name: Modify mongod.conf to change bindIp
    lineinfile:
      path: /etc/mongodb.conf
      regexp: '^bind_ip'
      line: 'bind_ip = 0.0.0.0'

# restart mongodb
  - name: Restart MongoDB service
    service:
      name: mongodb
      state: restarted

# enable mongodb
  - name: Enable mongod service
    service:
      name: mongodb
      enabled: yes


# go back to app maching, create an env var
# kill npm if needed
# restart the app should connect to dp
# share the posts page in the chat by lunch time
# complete documentation after lunch till 14:30

```