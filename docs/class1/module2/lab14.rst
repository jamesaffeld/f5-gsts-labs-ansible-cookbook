Changing the root password
==========================

Problem
-------

You need to change the password of the root user

Solution
--------

Use the ``bigip_user`` module.

#. *Type* the following into the ``playbooks/lab2.14.yaml`` file.

  ::

   ---

 - name: change root password
  hosts: bigip
  connection: local

  vars: 
    provider: 
      server: 10.1.1.4
      user: admin
      password: admin
      validate_certs: no

  tasks: 
    - name: change root password
      bigip_user: 
        username_credential: root
        password_credential: ChangedPassword1234
        provider: "{{ provider }}"

Run this playbook like so

  ::

   $ ansible-playbook -i inventory/hosts playbooks/lab2.14.yaml

Discussion
----------

The ``bigip_user`` module can manage user accounts across the system.

Specifying the users and credentials that you want to modify are done by
providing the ``username_credential`` or ``password_credential`` respectively.

In most cases, this module is idempotent. The way it achieves idempotency
for password changes is that it will attempt to log in to the REST API as
the specified ``username_credential``. If this succeeds, the password is
assumed to have already been changed, and it will not attempt to change
it again.

There is one case where this idempotency for passwords is not supported; the
``root`` account.

While we can change the root account via the REST API, there is no way to
subsequently log into the box as the ``root`` user to verify the password has
already been changed. Therefore, for the root user, and the root user only,
a ``changed`` event will be raised whenever you change its password.

Because of this, it is recommended that you put any Tasks that change the
root user account into their own, infrequently used, Playbooks.