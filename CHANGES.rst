Tequila-common
==============

Changes

v 0.9.2 on ?
-----------------------
* Pass packages to apt module as list rather than using squash_actions, which is more efficient and removes deprecation warning

v 0.9.1 on Sep 12, 2019
-----------------------

* Consistently use the `recommended module:options format
  <https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html#action-shorthand>`_
  for all tasks.
* Change ``include`` to ``include_tasks``.
* Fix search to be more specific for whether project user exists in /etc/ssh/sshrc.
* Requires at least Ansible 2.4

v 0.9.0 on Apr 1, 2019
----------------------

* Fix for Ansible 2.7.9: include direction on ufw tasks. (Became required
  in Ansible 2.7.8 or 2.7.9, not documented yet but see
  https://github.com/ansible/ansible/issues/53854)

v 0.8.9 on Sep 17, 2018
-----------------------

* Fix for Ubuntu 18+: python-software-properties package changed to
  software-properties-common.

v 0.8.8 on Aug 9, 2018
----------------------

* Fix for sftp: add --quiet to grep in /etc/sshrc.

v 0.8.7 on Aug 9, 2018
----------------------

* Fix for ssh agent forwarding: don't use setfacl on the socket unless
  the project user exists.

v 0.8.6 on Jul 30, 2018
-----------------------

* Some changes to help with ssh agent forwarding.


v 0.8.5 on Jul 24, 2018
-----------------------

* Configure the project user separately from the project name, as
  tequila-django does.


v 0.8.4 on Apr 19, 2018
-----------------------

* Revert the changes made in v0.8.3 as they can cause issues if
  RabbitMQ is installed, or if cloud-init is installed.


v 0.8.3 on Dec 15, 2017
-----------------------

* Set the hostname of the server and the localhost entry in /etc/hosts
  based on the hostname given in the inventory file.


v 0.8.2 on Nov 20, 2017
-----------------------

* Remove user accounts that are not within either ``users`` or the new
  ``unmanaged_users`` list.

  Before upgrading and deploying with this version, the servers
  covered under this role should be checked for accounts that have a
  home directory under /home/, are not stale developer users, but are
  not currently captured under the ``users`` variable.  These special
  users, if the assessment concludes that they need to remain, should
  be added to the ``unmanaged_users`` list.


v 0.8.1 on Oct 20, 2017
-----------------------

* Don't set ``ssl_dir`` in this role, since it's set in tequila-nginx.
  If you were previously not using tequila-nginx, but you WERE
  customizing ``ssl_dir``, then you'll need to create a task outside
  Tequila to create your customized ``ssl_dir`` directory.


v 0.8.0 on May 3, 2017
----------------------

* Initial release.
