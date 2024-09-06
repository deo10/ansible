# page 261-271
# Ansible Galaxy
Ansible Galaxy is a community-driven collection of Ansible roles
and collections, hosted by Red Hat at https://galaxy.ansible.com/.

ansible-galaxy role install -p roles/ arillso.motd

$ ansible-galaxy role init --init-path roles/ testrole
# - Role testrole was created successfully
$ tree roles/testrole/

├── README.md
├── defaults
│   └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
    └── main.yml

# Using conditions in your code

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

Initial version
---
- name: Play to patch only Fedora systems
  hosts: all
  become: true
  tasks:
  - name: Patch Fedora systems
      ansible.builtin.dnf:
        name: httpd
        state: latest
      when: ansible_facts['distribution'] == "Fedora"

v2
---
- name: Play to patch only Fedora systems
  hosts: all
  become: true
  tasks:
    - name: Patch Fedora systems
      yum:
        name: httpd
        state: latest
      when: (ansible_facts['distribution'] == "Fedora" and ansible_facts['distribution_major_version'] == "35")


---
- name: Play to test for hosts file in directory output
  hosts: localhost
  tasks:
    - name: Gather directory listing from local system
      ansible.builtin.shell: "ls -l"
      register: shellresult
    - name: Alert if we find a hosts file
      ansible.builtin.debug:
        msg: "Found hosts file!"
      when: '"hosts" in shellresult.stdout'

#here is a fragment of a playbook that will run a task only on Fedora 35 and newer

tasks:
- name: Only perform this task on Fedora 35 and later
  ansible.builtin.shell: echo "only on Fedora 35 and later"
  when: ansible_facts['distribution'] == "Fedora" and ansible_facts['distribution_major_version']|int >= 35

