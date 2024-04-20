# page 173-219

Defining Your Inventory

under the host_vars directory
web01.example.org.yml
---
https_port: 8444

under the group_vars directory
frontends.yml
---
https_port: 8443
lb_vip: lb.example.org

inventory
[frontends]
web01.example.org
web02.example.org
[apps]
app01.example.org
app02.example.org
[databases]
db01.example.org
db02.example.org

tree
.
├── group_vars
│ └── frontends.yml
├── host_vars
│ └── web01.example.org.yml
└── inventory
2 directories, 3 files

ansible -i inventory frontends -m ansible.builtin.debug -a "msg=\"Connecting to {{ lb_vip }}, listening on {{ https_port }}\""

