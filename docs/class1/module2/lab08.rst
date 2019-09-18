Provisioning ASM
================

Problem
-------

You need to provision ASM on the BIG-IP

Solution
--------

Use the ``bigip_provision`` module. 

#. *Type* the following into the ``playbooks/lab2.8.yaml`` file.

 ::

   ---
- name: an example provisioning playbook
  hosts: bigip
  connection: local

  vars: 
    provider: 
      validate_certs: no
      server: 10.1.1.4
      user: admin
      password: admin

  tasks: 
    - name: Provision ASM
      bigip_provision: 
         name: asm
      #   password: "{{ password }}"
      #   server: 10.1.1.4
      #   user: "{{ username }}"
      #   validate_certs: "{{ validate_certs }""
         provider: "{{ provider }}"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/site.yaml

Discussion
----------

The ``bigip_provision`` module can provision and de-provision modules from
the system.

This module will wait for a provisioning action to fully complete before
it allows the Playbook to proceed to the next task. This includes waiting
for the system to reboot and for MCPD to come online and be ready to take
new configuration.

All of the above also applies to ASM.

The level that all modules are provisioned at is ``nominal`` by default. This
can be changed using the ``level`` argument. Valid choices are,

* ``dedicated``
* ``nominal``
* ``minimum``

This module is smart enough to known when certain modules require specific
provisioning levels. For example, vCMP is always ``dedicated``.