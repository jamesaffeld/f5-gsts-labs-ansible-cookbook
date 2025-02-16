Installing an iApp template on BIG-IP
=====================================

Problem
-------

You need to install an App Services Integration iApp

Solution
--------

Use the ``bigip_iapp_template`` module.

#. iApps are deprecated - skip this lab

 ::

   ---

   - name: An example iApp template playbook
     hosts: bigip
     connection: local

     vars:
       validate_certs: no
       username: admin
       password: admin

     tasks:
       - name: Add the iApp
         bigip_iapp_template:
           content: "{{ lookup('file', '../files/appsvcs_integration_v2.0.004.tmpl') }}"
           password: "{{ password }}"
           server: 10.1.1.4
           state: present
           user: "{{ username }}"
           validate_certs: "{{ validate_certs }}"

Run this playbook, from the ``lab2.6`` directory like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/site.yaml

Discussion
----------

The ``bigip_iapp_template`` module can manage the TCL iApps that are
installed on the remote BIG-IP.

Most arguments to the module are unnecessary because the module will
attempt to parse the iApp itself to determine the necessary values.

Nevertheless, if you do provide the values, they will **override** what
is in content of the iApp itself.
