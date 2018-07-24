tequila-common
==============

This repository holds an `Ansible <http://www.ansible.com/home>`_ role
that is installable using ``ansible-galaxy``.  This role contains
tasks that are common across multiple roles for a Django deployment.
It exists primarily to support the `Caktus Django project template
<https://github.com/caktus/django-project-template>`_.

More complete documenation can be found in `caktus/tequila
<https://github.com/caktus/tequila>`_.


License
-------

This Ansible role is released under the BSD License.  See the `LICENSE
<https://github.com/caktus/tequila-common/blob/master/LICENSE>`_ file
for more details.


Contributing
------------

If you think you've found a bug or are interested in contributing to
this project check out `tequila-common on Github
<https://github.com/caktus/tequila-common>`_.

Development sponsored by `Caktus Consulting Group, LLC
<http://www.caktusgroup.com/services>`_.


Installation
------------

Create an ``ansible.cfg`` file in your project directory to tell
Ansible where to install your roles (optionally, set the
``ANSIBLE_ROLES_PATH`` environment variable to do the same thing, or
allow the roles to be installed into ``/etc/ansible/roles``).
You should also enable ssh pipelining for performance, and might
optionally want to enable ssh agent forwarding.::

    [defaults]
    roles_path = deployment/roles/
    [ssh_connection]
    pipelining = True
    ssh_args = -o ForwardAgent=yes -o ControlMaster=auto -o ControlPersist=60s -o ControlPath=/tmp/ansible-ssh-%h-%p-%r

Create a ``requirements.yml`` file in your project's deployment
directory ::

    ---
    # file: deployment/requirements.yml
    - src: https://github.com/caktus/tequila-common
      version: X.Y.Z

Run ``ansible-galaxy`` with your requirements file ::

    $ ansible-galaxy install -r deployment/requirements.yml

or, alternatively, run it directly against the url ::

    $ ansible-galaxy install git+https://github.com/caktus/tequila-common

The project then should have access to the ``tequila-common`` role in
its playbooks.


Variables
---------

The following variables are made use of by the ``tequila-common``
role:

- ``project_name`` **required**
- ``project_user`` **default:** ``{{ project_name }}``
- ``env_name`` **required** e.g. ``'staging'``
- ``users`` **default:** empty list
- ``project_user`` **default:** ``"{{ project_name }}"``
- ``unmanaged_users`` **default:** empty list
- ``root_dir`` **default:** ``"/var/www/{{ project_name }}"``
- ``log_dir`` **default:** ``"{{ root_dir }}/log"``
- ``public_dir`` **default:** ``"{{ root_dir }}/public"``
- ``static_dir`` **default:** ``"{{ root_dir }}/public/static"``
- ``media_dir`` **default:** ``"{{ root_dir }}/public/media"``
- ``ssh_dir`` **default:** ``"/home/{{ project_user }}/.ssh"``
- ``use_newrelic`` **default:** ``false``
- ``new_relic_license_key`` **required if use_newrelic = true**
- ``subproject`` **default:** ``false`` - for multiple project deploys,
   this can be set true for all but one project, so things that only
   need to be done once on the server can be skipped for all but one
   project.

The ``users`` variable is meant to be a list of dicts, where each dict
represents a developer.  There are two required keys for each dict:
``name``, for the username of the developer, and ``public_keys``, with
a list of the user's public ssh keys.  So, a typical definition for
``users`` in an actual project might look like ::

    users:
      - name: user1
        public_keys:
          - "ssh-rsa AAAAAA user1@example.com"
      - name: user2
        public_keys:
          - "ssh-rsa AAAAAA user2@example.com"
      - name: user3
        public_keys:
          - "ssh-rsa AAAAAA user3@example.com"

The ``unmanaged_users`` variable is a list that is intended to hold
any special-case users that have been created by other means, that it
is not desirable to remove as stale.  These users will not be created
by tequila-common if they do not already exist.  It is probably a good
idea to include ``vagrant`` in this list, if you are making use of
Vagrant box(es) in your project.  This variable should probably be set
in your project-wide variables file
(e.g. deployment/playbooks/group_vars/all/project.yml), and should
look something like ::

    unmanaged_users:
      - special_user1
      - special_user2
      - special_user3

All users that have a directory under /home/ and that are not in
either ``users`` or ``unmanaged_users`` (not counting the special user
that owns the files from the codebase) will be removed on each run of
this role.  However, the users' files and directories will not be
removed.


SSH Agent Forwarding
--------------------

SSH agent forwarding is optional, but can be handy for things like
using private git repositories or node packages during the deploy.

To enable it, include "-o ForwardAgent=yes " in
``ssh_args`` in ``ansible.cfg``, as in the example above.

If you don't want to have ssh agent forwarding, just change it to
"-o ForwardAgent=no". That'll always block agent forwarding during
deploys.  Or leave that option out of the configuration, which'll
leave it up to your users to decide whether to forward their
ssh agents based on their own ssh configurations.
