# page 1-100

# ansible versions and ansible install
ansible-core - 2.17
ansible-playbook
https://github.com/ansible/ansible

# Display gathered facts:
ansible webservers -i /etc/ansible/hosts -m ansible.builtin.setup | less
filter
ansible webservers -i /etc/ansible/hosts -m ansible.builtin.setup -a "filter=ansible_distribution*"

# console common tasks
Copy a file from the Ansible control node to all managed nodes in the
webservers group with the following command:
$ ansible webservers -m ansible.builtin.copy -a "src=/etc/hostname dest=/tmp/hostname"

Create a new directory on all hosts in the webservers inventory group,
and create it with specific ownership and permissions, as follows:
$ ansible webservers -m ansible.builtin.file -a "dest=/tmp/newpath mode=777 owner=james group=james state=directory"

Delete a specific directory from all hosts in the webservers group with
the following command:
$ ansible webservers -m ansible.builtin.file -a "dest=/tmp/newpath state=absent"

Install the apache2 package with apt if it is not already present—if it is
present, do not update it. This command assumes that the user account on the managed node
can perform passwordless sudo commands:
$ ansible webservers -m ansible.builtin.apt -a "name=apache2 state=present" --become

The following command is similar to the previous one, except that
changing state=present to state=latest causes Ansible to
install the (latest version of the) package if it is not present, and update it
to the latest version if it is present:
$ ansible webservers -m ansible.builtin.apt -a "name=apache2 state=latest" --become

Display all facts about all the hosts in your inventory (warning—this will
produce a lot of JSON!):
$ ansible all -m ansible.builtin.setup

