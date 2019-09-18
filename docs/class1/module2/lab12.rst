Saving your configuration
=========================

Problem
-------

You need to save the running configuration of a BIG-IP

Solution
--------

Use the ``bigip_config`` module.

#. *Type* the following into the ``playbooks/lab2.12.yaml`` file.

  ::

   ---

- name: an example config saving playbook
  hosts: bigip
  connection: local

  vars: 
    provider: 
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no

  tasks: 
    - name: save running configuration
      bigip_config: 
         save: yes
         provider: "{{ provider }}"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.12.yaml

Discussion
----------

The ``bigip_config`` module has several purposes, one of which is
to save your configuration.

In addition to this, you can merge an existing configuration that you
might have (in the SCF format) into the running configuration using
the ``merge_content`` argument..

You can also reset the running configuration, should you so desire,
using the ``reset`` argument.