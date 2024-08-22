# page 241-261
# Understanding roles – the playbook organizer

The subdirectories that can exist inside of a role are as follows:
# tasks:
This is the most common directory to find in a role, and it contains all of the Ansible tasks that the role should perform.
# handlers:
All handlers used in the role should go into this directory.
# defaults:
All default variables for the role go in here.
# vars:
These are other role variables – these override those declared in the defaults/directory, as they are higher up the precedence order.
# files:
Files needed by the role should go in here – for example, any
configuration files that need to be deployed to the target hosts.
# templates:
Distinct from the files/ directory, this directory should
contain all templates used by the role.
# meta:
Any metadata needed for the role goes in here. For example, roles
are normally executed in the order they are called from the parent
playbook; however, sometimes, a role will have dependency roles that need to be run first, and if this is the case, they can be declared within this directory.

.
├── hosts
├── roles
│ └── installapache
│ └── tasks
│ ├── fedora.yml
│ ├── main.yml
│ └── ubuntu.yml
└── site.yml

site.yml
---
- name: Install Apache using a role
  hosts: frontends
  become: true
  roles:
    - installapache


roles/installapache/tasks/main.yml
---
- name: import a tasks based on OS platform
  import_tasks: fedora.yml
  when: ansible_distribution == 'Fedora'
- import_tasks: ubuntu.yml
  when: ansible_distribution == 'Ubuntu'

roles/installapache/tasks/fedora.yml
---
- name: Install Apache using dnf
  ansible.builtin.dnf:
    name: httpd
    state: latest
- name: Start the Apache server
  ansible.builtin.service:
    name: httpd
    state: started

roles/installapache/tasks/ubuntu.yml
---
- name: Install Apache using apt
  ansible.builtin.apt:
    name: apache2
    state: latest
- name: Start the Apache server
  ansible.builtin.service:
    name: apache2
    state: started

Role-based variables can go in one of two locations:
defaults/main.yml
vars/main.yml

site.yml
---
- name: Role variables and meta playbook
  hosts: web01.example.org
  roles:
    - platform

roles/platform/meta/main.yml
---
dependencies:
- role: linuxtype
    type: "fedora"
- role: linuxtype
    type: "ubuntu"

roles/linuxtype/meta/main.yml
---
dependencies:
- role: version
- role: network

roles/version/meta/main.yml
---
allow_duplicates: true

roles/version/tasks/main.yml
---
- name: Print type variable
    ansible.builtin.debug:
      var: type

roles/network/meta/main.yml
---
allow_duplicates: true

roles/network/tasks/main.yml
---
- name: Print type variable
    ansible.builtin.debug:
      var: type

.
├── hosts
├── roles
│   ├── linuxtype
│   │   └── meta
│   │       └── main.yml
│   ├── network
│   │   ├── meta
│   │   │   └── main.yml
│   │   └── tasks
│   │       └── main.yml
│   ├── platform
│   │   └── meta
│   │       └── main.yml
│   └── version
│       ├── meta
│       │   └── main.yml
│       └── tasks
│           └── main.yml
└── site.yml
