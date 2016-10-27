tequila-common
==============

This repository holds an `Ansible <http://www.ansible.com/home>`_ role
that is installable using ``ansible-galaxy``.  This role contains
tasks that are common across multiple roles for a Django deployment.
It exists primarily to support the `Caktus Django project template
<https://github.com/caktus/django-project-template>`_.


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
allow the roles to be installed into ``/etc/ansible/roles``) ::

    [defaults]
    roles_path = deployment/roles/

Create a ``requirements.yml`` file in your project's deployment
directory ::

    ---
    # file: deployment/requirements.yml
    - src: https://github.com/caktus/tequila-common
      version: 0.1.0

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
- ``users`` **default:** empty list
- ``root_dir`` **default:** ``"/var/www/{{ project_name }}"``
- ``log_dir`` **default:** ``"{{ root_dir }}/log"``
- ``public_dir`` **default:** ``"{{ root_dir }}/public"``
- ``static_dir`` **default:** ``"{{ root_dir }}/public/static"``
- ``media_dir`` **default:** ``"{{ root_dir }}/public/media"``
- ``ssh_dir`` **default:** ``"/home/{{ project_name }}/.ssh"``
- ``ssl_dir`` **default:** ``"{{ root_dir }}/ssl"``
- ``use_newrelic`` **default:** ``false``
- ``new_relic_license_key`` **required if use_newrelic = true**

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
