Tequila-common
==============

Changes

v 0.8.X on XXX XX, 2018
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
