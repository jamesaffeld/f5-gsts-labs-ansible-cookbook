Creating a virtual server on BIG-IP
===================================

Problem
-------

You need to create a virtual server, associated with a pool, on a BIG-IP

Solution
--------

Use the ``bigip_virtual_server`` module.

#. *Type* the following into the ``playbooks/lab2.5.yaml`` file.

 ::

   ---

- name: an example pool playbook - create VS 
  hosts: bigip
  connection: local

  vars:
    provider:
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no
 
  tasks:
     - name: create VS     
       bigip_virtual_server:
         destination: 10.1.1.100
         pool: web-servers
         port: 80    
         name: vip-1
         snat: Automap
         profiles:
           - http
           - clientssl
         provider: "{{ provider }}"
         description: "VS added by Ansible"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.5.yaml

Discussion
----------

The ``bigip_virtual_server`` module can configure a number of attributes for a
virtual server. At a minimum, the ``name`` is required.

This module is idempotent. Therefore, you can run it over and over again
and so-long as no settings have been changed, this module will report no
changes.

Several arguments, such as ``policies`` and ``profiles`` take a list of values.
If you update this list of values, it will be reflected on the virtual
server’s configuration. This includes removing items from these lists.

As an example, if you have four items in the ``profile`` list, and then you
remove one, this will cause the virtual server to be reconfigured to only
have three profiles.