# page 163-173

Jinja2

More examples on - https://jinja.palletsprojects.com/en/3.1.x/

---
- name: Jinja2 filtering demo 2
  hosts: localhost
  vars:
    tags:
    - key: job
      value: developer
    - key: language
      value: java
  tasks:
    - name: Filter the tags variable through items2 dict
      ansible.builtin.debug:
        msg: '{{ tags | items2dict }}'


---
- name: Jinja2 filtering demo 3
  hosts: localhost
  vars:
    ping_value: "{{ lookup('file', '/etc/hosts') }}"
  tasks:
    - name: Display the values obtained from the file lookup
      ansible.builtin.debug:
        msg: "ping value is {{ ping_value }}"


# Add some quotation in the shell
- shell: echo {{ string_value | quote }}
# Concatenate a list into a specific string
{{ list | join("$") }}
# Obtain the file name of a specific file path
{{ path | basename }}
# Retrieve the directory from a full path
{{ path | dirname }}
# Get the directory from a specific windows path
{{ path | win_dirname }}

