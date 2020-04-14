Filesystem requirements Ansible role
====================================

Check size and/or inodes filesystem requirements.

Typically use as pre-install check for anything else, regardless of partitioning and with the following logic:
  - first, for each requirement, if path does not exist, identify longest existing sub path and add requirements for it's mount point
  - second, resulting mount point requirements are checked against values from hostvars, leading to failure if unsatisfied

Requirements
------------

None

Role Variables
--------------

- `filesystem_requirements`: list of requirements to be satified, each requirement being a dict with keys: path, size, inodes

- `req_grp_verb`: verbosity level for main structures debug

- `req_verb`: verbosity level for in loop requirements debug

Dependencies
------------

None

Example Playbook
----------------

    # empty requirements should luckily be satisfied
    - hosts: servers
      roles:
        - role: gbaudot.ansible-fs-requirements

    # almost as useless as above, a path without requirements should be satisfied too
    - hosts: some-servers
      roles:
        - role: gbaudot.ansible-fs-requirements
          filesystem_requirements:
            - path: /no/matter/where

    # an hypothetic web app tied to a DB and known to generate many temp files at installation
    - hosts: web-app-servers
      roles:
        - role: gbaudot.ansible-fs-requirements
          filesystem_requirements:
            - path: /var/www/my-web-app/some-version
              size: 800 M
              inodes: 10 KB
            - path: /var/lib/mysql/my_database
              size: 100M
            - path: /tmp
              inodes: 30KB

License
-------

BSD

Author Information
------------------

None
