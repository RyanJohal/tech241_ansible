# Nginx and reverse proxy YAML playbook
```
# Why YAML, able to apply in multiple places
# Playbooks are used because they are reuseable
# YAML file start with --- three dashes
# Create a playbook to install nginx in web node
---

# which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes

# admin access
  become: true
# add the instructions - install nginx - web
  tasks:
  - name: Installing Nginx
    apt: pkg=nginx state=present
# ensure the status of nginx is actively running
# adhoc command to check the status

# configure reverse proxy 

  - name: Customize Nginx default configuration file
    lineinfile:
      path: /etc/nginx/sites-available/default
      regexp: '^(\s*try_files.*)$'
      line: '        proxy_pass http://localhost:3000;'

# restart nginx

  - name: Restart Nginx service
    service:
      name: nginx
      state: restarted

```

# Automating launch of app
```
---

# Which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes
# admin access (sudo)
  become: true
# add the instructions  -  install node 12 with pm2 and run the app
  tasks:
  - name: Installing node v 12the gog key for nodejs
    apt_key:
      url: "https://deb.nodesource.com/gpgkey/nodesource.gpg.key"
      state: present

  - name: Add NodeSource repository
    apt_repository:
      repo: "deb https://deb.nodesource.com/node_12.x {{ ansible_distribution_release }} main"
      state: present

  - name: Install Node.js
    apt:
      name: nodejs
      state: present
      update_cache: yes

  - name: Install PM2
    npm:
      name: pm2
      global: yes
      state: present

  - name: Clone the Git repository
    git:
      repo: https://github.com/RyanJohal/tech241_sparta_app
      dest: /home/ubuntu/repo
    become: yes

  - name: Install app dependencies
    command: npm install
    args:
      chdir: "repo/app"

  - name: Stop PM2 processes
    shell: pm2 kill

  - name: Start the Node.js app
    command: pm2 start app.js
    args:
      chdir: "repo/app/"

```


# Full script
```
---

# which host to perform the task
- hosts: web

# see the logs by gathering facts
  gather_facts: yes

# admin access
  become: true
# add the instructions - install nginx - web
  tasks:
  - name: Installing Nginx
    apt: pkg=nginx state=present

# configure reverse proxy

  - name: Customize Nginx default configuration file
    lineinfile:
      path: /etc/nginx/sites-available/default
      regexp: '^(\s*try_files.*)$'
      line: '        proxy_pass http://localhost:3000;'

# restart nginx

  - name: Restart Nginx service
    service:
      name: nginx
      state: restarted

  - name: Installing node v 12the gog key for nodejs
    apt_key:
      url: "https://deb.nodesource.com/gpgkey/nodesource.gpg.key"
      state: present

  - name: Add NodeSource repository
    apt_repository:
      repo: "deb https://deb.nodesource.com/node_12.x {{ ansible_distribution_release }} main"
      state: present

  - name: Install Node.js
    apt:
      name: nodejs
      state: present
      update_cache: yes

  - name: Install PM2
    npm:
      name: pm2
      global: yes
      state: present

  - name: Clone the Git repository
    git:
      repo: https://github.com/RyanJohal/tech241_sparta_app
      dest: /home/ubuntu/repo
    become: yes

  - name: Install app dependencies
    command: npm install
    args:
      chdir: "repo/app"

  - name: Seeding database
    command: node seeds/seed.js
    args:
      chdir: /home/ubuntu/repo/app/

  - name: Stop PM2 processes
    shell: pm2 kill

  - name: Start the Node.js app
    command: pm2 start app.js
    args:
      chdir: "repo/app/"


``` 