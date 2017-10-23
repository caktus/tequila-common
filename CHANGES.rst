Tequila-common
==============

Changes

v 0.8.1 on Oct 20, 2017
-----------------------

* Don't set ``ssl_dir`` in this role, since it's set in tequila-nginx.
  If you were previously not using tequila-nginx, but you WERE
  customizing ``ssl_dir``, then you'll need to create a task outside
  Tequila to create your customized ``ssl_dir`` directory.

v 0.8.0 on May 3, 2017
----------------------

* Initial release.
