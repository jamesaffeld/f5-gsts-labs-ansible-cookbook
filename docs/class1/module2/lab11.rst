Creating a new partition
========================

Problem
-------

You need to create separate partitions on the BIG-IP for different
tenants or resource management

Solution
--------

Use the ``bigip_partition`` module.

#. *Type* the following into the ``playbooks/site.yaml`` file.

  ::

   ---

 - name: an example partition playbook
  hosts: bigip
  connection: local

  vars: 
    provider: 
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no

  tasks: 
    - name: create partition
      bigip_partition: 
        name: my-partition
        provider: "{{ provider }}"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.11.yaml

Discussion
----------

The ``bigip_partition`` module can manage partitions on the system.

Partitions can be used in other modules after they are created. To use them
in modules that support them, provide the ``partition`` parameter.

Some modules, such as ``bigip_selfip`` allow you to modify resources that can
exist in another partition. In you want to do this, name those resources
explicitly using their full path (i.e., ``/foo/vlan1``). If you do not name the
full path, the module in question will assume the partition that is supplied
in the ``partition`` argument. By default, this is ``Common``.

At the time of this writing, partitions can **not** be removed until all of the
resources under them have been removed. We realize this is a source of pain,
but there is truly no supported way of removing a partition and all of its
resources. A future update will provide a workaround.
