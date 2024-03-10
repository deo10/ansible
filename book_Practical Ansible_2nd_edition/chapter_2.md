# page 100-163

# Simple playbook
---
- hosts: web2
  become: yes
  tasks:
  - name: Install/Update to the latest of Apache Web Server
    ansible.builtin.apt:
      name: apache2
      state: latest
    notify:
    - Restart the Apache Web Server
  handlers:
  - name: Restart the Apache Web Server
    ansible.builtin.service:
      name: apache2
      state: restarted

ansible-playbook -i hosts --limit web* playbook.yml
#limit execution for hosts with web in name

The central configuration file (which impacts the behavior of Ansible for all users on the system)
can be found at /etc/ansible/ansible.cfg.

ansible-config list

#command output
ACTION_WARNINGS:
default: true
description:
- By default Ansible will issue a warning when re
ceived from a task action (module
or action plugin)
- These warnings can be silenced by adjusting thi
s setting to False.
env:
- name: ANSIBLE_ACTION_WARNINGS
ini:
- key: action_warnings
section: defaults
name: Toggle action warnings
type: boolean
version_added: '2.5'
AGNOSTIC_BECOME_PROMPT:
default: true
description: Display an agnostic become prompt in
stead of displaying a prompt containing
the command line supplied become method
env:
- name: ANSIBLE_AGNOSTIC_BECOME_PROMPT
ini:
- key: agnostic_become_prompt
section: privilege_escalation
name: Display an agnostic become prompt
type: boolean
version_added: '2.5'
yaml:
key: privilege_escalation.agnostic_become_prompt

ansible-config dump
#command output
ACTION_WARNINGS(default) = True
AGNOSTIC_BECOME_PROMPT(default) = True
ANSIBLE_CONNECTION_PATH(default) = None
ANSIBLE_COW_ACCEPTLIST(default) = ['bud-frogs', 'bu
nny', 'cheese', 'daemon', 'default', 'dragon', 'ele
phant-in-snake', 'elephant', 'eyes', 'hellok>
ANSIBLE_COW_PATH(default) = None
ANSIBLE_COW_SELECTION(default) = default
ANSIBLE_FORCE_COLOR(default) = False
ANSIBLE_HOME(default) = /home/james/.ansible
…
CONFIG_FILE() = None
…

# ansible vars

---
- name: Display redis variables
  hosts: all

  vars:
    redis:
      server: cacheserver01.example.com
      port: 6379
      slaveof: cacheserver02.example.com

  tasks:
    - name: Display the redis port
      ansible.builtin.debug:
        msg: "The redis port for {{ redis.server }} is {{ redis.port }}"

#another example
---
- name: Parse the employees playbook
  hosts: localhost
  vars_files:
    - employees-all.yml

  tasks:
    - name: Print the variables
      debug:
        var: employees


#employees-all.yml
---
servers:
  - frontend
  - backend
  - database
  - cache
employees:
  - name: daniel
    fullname: Daniel Oh
    role: DevOps Evangelist
    level: Expert
    skills:
      - Kubernetes
      - Microservices
      - Ansible
      - Linux Container
  - name: michael
    fullname: Michael Smiths
    role: Enterprise Architect
    level: Advanced
    skills:
      - Cloud
      - Middleware
      - Windows
      - Storage
    Speciality: |
      Agile methodology
      Cloud-native app development practices
      Advanced enterprise DevOps practices