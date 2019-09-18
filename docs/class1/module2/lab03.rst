Creating a pool with a physical node
========================

Problem
-------

You need to create a pool with a node.

Solution
--------

Use the ``bigip_node`` module.

#. *Type* the following into the ``playbooks/lab2.4.yaml`` file.

 ::

   ---
- name: an example pool playbook - add nodes
  hosts: bigip
  connection: local

  vars:
    provider:
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no
 
  tasks:
     - name: add nodes to web servers pool      
       bigip_pool_member:
#         host: 10.1.20.11 
         pool: web-servers
         aggregate: 
           - host: 10.1.20.11         
             port: 80
         reuse_nodes: yes

         provider: "{{ provider }}"
         description: "webserver-1 added by Ansible"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.3.yaml

Discussion
----------

The ``bigip_node`` module can configure physical device addresses that can
later be added to pools. At a minimum, the ``name`` is required. Additionally,
either the ``address`` or ``fqdn`` parameters are also required when creating
new nodes.

This module can take hostnames using the ``fqdn`` parameter. You may not specify
both the ``address`` and ``fqdn``.
