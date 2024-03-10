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
