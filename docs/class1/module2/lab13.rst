Waiting for your device to (re)boot
===================================

Problem
-------

You need to reboot the BIG-IP and wait for it to come back up

Solution
--------

Reboot the device with ``bigip_command``, then use ``bigip_wait`` to wait
for the device to come back up and be ready to take configuration.

#. *Type* the following into the ``playbooks/lab2.13.yaml`` file.

  ::

   ---

 - name: an example reboot playbook
  hosts: bigip
  connection: local

  vars: 
    provider: 
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no

  tasks: 
   - name: Reboot BIG-IP
     bigip_command: 
       commands: tmsh reboot
       provider: "{{ provider }}"
       #ignore_errors: true

   - name: wait for shutdown to happen
     pause: 
      seconds: 90

   - name: wait for BIG-IP to actually be ready
     bigip_wait:
       provider: "{{ provider }}"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.13.yaml

Discussion
----------

Waiting for the BIG-IP to be available is actually a really difficult thing
to do. It gets better in later versions of BIG-IP (13.1 and beyond). For those
and all the earlier releases (back to 12.0.0) you can use this module.

This module will not return until the BIG-IP is ready to take configuration.
This means that it will wait for,

* mcpd
* iControl REST
* ASM
* vCMP

Notice that I mentioned several features that themselves are problematic to
wait for. This module will accommodate them.

Once this module returns (and Ansible moves on to the next Task) you will be
able to use any F5 Ansible module that would change the configuration.