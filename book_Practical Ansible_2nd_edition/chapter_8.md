# page 271-281
# Repeating tasks with loops

---
- name: Simple loop demo play
  hosts: web01.example.org
  tasks:
  - name: Echo a value from the loop
    ansible.builtin.command: echo "{{ item }}"
    loop:
      - 1
      - 2
      - 3
      - 4
      - 5
      - 6

---
- name: Simple loop demo play
  hosts: web01.example.org
  tasks:
  - name: Echo a value from the loop
    ansible.builtin.command: echo "{{ item }}"
    loop:
      - 1
      - 2
      - 3
      - 4
      - 5
      - 6
    when: item|int > 3  # will skip first 3 items in the list


---
- name: Simple loop demo play
  hosts: web01.example.org
  tasks:
  - name: Echo a value from the loop
    ansible.builtin.command: echo "{{ item }}"
    loop:
      - 1
      - 2
      - 3
      - 4
      - 5
      - 6
    when: item|int > 3
    register: loopresult
  - name: Print the results from the loop
    ansible.builtin.debug:
      var: loopresult


---
- name: Play to demonstrate nested loops
  hosts: localhost
  tasks:
  - name: Outer loop
    ansible.builtin.include_tasks: loopsubtask.yml
    loop:
      - a
      - b
      - c
    loop_control:
      loop_var: second_item


loopsubtask.yml:
---
- name: Inner loop
    ansible.builtin.debug:
      msg: "second item={{ second_item }} first item=
  {{ item }}"
    loop:
      - 100
      - 200
      - 300

$ ansible-playbook loopmain.yml


