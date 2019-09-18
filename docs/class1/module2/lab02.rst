Writing once, re-using many times
=================================

Problem
-------

You want to specify the values for user/pass and validate_certs only once
but re-use them throughout your tasks

Solution
--------

Use variables.

#. *Type* the following into the ``playbooks/lab2.2.yaml`` file.

 ::

   ---
- name: an example pool playbook
  hosts: bigip
  connection: local

  vars:
    provider:
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no
 
  tasks:
     - name: Create node for web servers pool      
       bigip_node: 
         name: web-servers_node1
         address: 10.1.20.11  
         provider: "{{ provider }}"
         description: "Created by Ansible"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.2.yaml

Discussion
----------

Variables are one of the ways in which you can set a value once and re-use it
across many tasks in your Play.

It should be noted that variables **do not** survive across Plays. Therefore,
if you need to use them in multiple plays, it is better to put them in a
``host_vars`` or ``group_vars`` file.

Variables are identified by their double curly braces (``{{`` and ``}}``). The value
in-between these braces is the variable name.

Notice how we set our variables at the top of the play in the ``vars`` section.
This is a special section of the Playbook where you can specify variable data
that will be used across this Play and this Play only.

When using variables, they *must* be wrapped in double quotes. You can see this
in the ``bigip_pool`` task for the ``password``, ``user``, and ``validate_certs``
arguments.