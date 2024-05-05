# page 219-241

Playbooks and Roles

inventory
[frontends]
web01.example.org https_port=8443
web02.example.org http_proxy=proxy.example.org
[frontends:vars]
ntp_server=ntp.web.example.org
proxy=proxy.web.example.org
[apps]
app01.example.org
app02.example.org
[webapp:children]
frontends
apps
[webapp:vars]
proxy_server=proxy.webapp.example.org
health_check_retry=3
health_check_interval=60

myplabook.yml
---
- hosts: frontends
  remote_user: james
  tasks:
  - name: simple connection test
    ansible.builtin.ping:
    remote_user: james
  - name: run a simple command
    ansible.builtin.shell: /bin/ls -al /nonexistent
    ignore_errors: True

example.yml
---
- name: Handler demo 1
  hosts: web01.example.org
  gather_facts: no
  become: yes
  tasks:
  - name: Update Apache configuration
    ansible.builtin.template:
      src: template.j2
      dest: /etc/apache2/apache2.conf
    notify: Restart Apache
  handlers:
  - name: Restart Apache
    ansible.builtin.service:
      name: apache2
      state: restarted

example_2.yml
---
- name: Handler demo 1
  hosts: web01.example.org
  gather_facts: no
  become: yes
  handlers:
  - name: restart timesyncd
    ansible.builtin.service:
      name: systemd-timesyncd.service
      state: restarted
    listen: "restart all services"
  - name: restart apache
    ansible.builtin.service:
      name: apache2.service
      state: restarted
    listen: "restart all services"
  tasks:
  - name: restart all services
    ansible.builtin.command: echo "this task will restart all services"
    notify: "restart all services"

play_example.yml
---
- name: Play 1 - configure the frontend servers
  hosts: frontends
  become: yes
  tasks:
  - name: Install the Apache package
    ansible.builtin.apt:
      name: apache2
      state: latest
  - name: Start the Apache server
    ansible.builtin.service:
      name: apache2
      state: started
- name: Play 2 - configure the application servers
  hosts: apps
  become: true
  tasks:
  - name: Install Tomcat
    ansible.builtin.apt:
      name: tomcat9
      state: latest
  - name: Start the Tomcat server
    ansible.builtin.service:
      name: tomcat9
      state: started