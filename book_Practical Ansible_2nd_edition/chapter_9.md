# page -281
# Grouping tasks using blocks

Install the package for the Apache web server
Install a templated configuration
Start the appropriate service


Only Fedora servers will be used to install Apache
---
- name: Conditional block play
  hosts: all
  become: true
  tasks:
  - name: Install and configure Apache
    block:
      - name: Install the Apache package
        ansible.builtin.dnf:
          name: httpd
          state: installed
      - name: Install the templated configuration to a dummy location
        ansible.builtin.template:
          src: templates/src.j2
          dest: /tmp/my.conf
      - name: Start the httpd service
        ansible.builtin.service:
          name: httpd
          state: started
          enabled: True
    when: ansible_facts['distribution'] == 'Fedora'



---
- name: Play to demonstrate block error handling
  hosts: frontends
  tasks:
  - name: block to handle errors
    block:
      - name: Perform a successful task
        ansible.builtin.debug:
          msg: 'Normally executing....'
      - name: Deliberately create an error
        ansible.builtin.command: /bin/whatever
      - name: This task should not run if the previous one results in an error
        ansible.builtin.debug:
          msg: 'Never print this message if the above command fails!!!!'
    rescue:
      - name: Catch the error (and perform recovery actions)
        ansible.builtin.debug:
          msg: 'Caught the error'
      - name: Deliberately create another error
        ansible.builtin.command: /bin/whatever
      - name: This task should not run if the previous one results in an error
        ansible.builtin.debug:
          msg: 'Do not print this message if theabove command fails!!!!'
    always:
      - name: This task always runs!
        ansible.builtin.debug:
          msg: "Tasks in this part of the play will be ALWAYS executed!!!!"


# The flow of execution is as follows:
1. All tasks in the block section are executed normally, in the sequence in
which they are listed.
2. If a task in the block results in an error, no further tasks in the block
are run:
   1. Tasks in the rescue section start to run in the order they are listed
   2. Tasks in the rescue section do not run if no errors result from the
   block tasks
3. If an error results from a task being run in the rescue section, no further
rescue tasks are executed, and execution moves on to the always
section.
4. Tasks in the always section are always run, regardless of any errors in
either the block or rescue sections. They even run when no errors are
encountered.

# Ansible has two special variables, which contain information you might find
useful in the rescue block to perform your recovery actions:

   ansible_failed_task: This is a dictionary containing details of the
task from block that failed, causing us to enter the rescue section. You
can explore this by displaying its contents using
ansible.builtin.debug, so, for example, the name of the failing
task can be obtained from ansible_failed_task.name.

   ansible_failed_result: This is the result of the failed task and
behaves the same as if you had added the register keyword to the
failing task. This saves you from having to add register to every single
task in the block if it fails.

