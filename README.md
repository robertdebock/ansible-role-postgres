postgres
=========

<img src="https://docs.ansible.com/ansible-tower/3.2.4/html_ja/installandreference/_static/images/logo_invert.png" width="10%" height="10%" alt="Ansible logo" align="right"/>
<a href="https://travis-ci.org/robertdebock/ansible-role-postgres"> <img src="https://travis-ci.org/robertdebock/ansible-role-postgres.svg?branch=master" alt="Build status"/></a> <img src="https://img.shields.io/ansible/role/d/42173"/> <img src="https://img.shields.io/ansible/quality/42173"/>

Install and configure postgres on your system.

Example Playbook
----------------

This example is taken from `molecule/resources/playbook.yml`:
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  roles:
    - role: robertdebock.postgres
      postgres_databases:
        - name: test_db
      postgres_users:
        - name: test_user_with_pass
          password: "MyCoMpLeXpAsSwOrD"
        - name: test_user_with_db
          password: "MyCoMpLeXpAsSwOrD"
          db: test_db
      postgres_hba_entries:
        - type: local
          database: all
          user: all
          method: peer
        - type: host
          database: all
          user: all
          address: 127.0.0.1/32
          method: ident
        - type: host
          database: all
          user: all
          address: ::1/128
          method: ident
        - type: local
          database: replication
          user: all
          method: peer
        - type: host
          database: replication
          user: all
          address: 127.0.0.1/32
          method: ident
        - type: host
          database: replication
          user: all
          address: ::1/128
          method: ident
```

The machine you are running this on, may need to be prepared.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - robertdebock.bootstrap
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for postgres

postgres_settings:
  - name: listen_addresses
    value: 5432
  - name: max_wal_size
    value: 1GB
  - name: min_wal_size
    value: 80MB
  - name: log_timezone
    value: UCT
  - name: datestyle
    value: "'iso, ymd'"
  - name: timezone
    value: UCT
  - name: default_text_search_config
    value: "'pg_catalog.english'"
```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap

```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/postgres.png "Dependency")


Compatibility
-------------

This role has been tested on these [container images](https://hub.docker.com/):

|container|tag|allow_failures|
|---------|---|--------------|
|amazonlinux|1|no|
|amazonlinux|latest|no|
|alpine|latest|no|
|alpine|edge|yes|
|debian|stable|yes|
|debian|unstable|yes|
|debian|latest|no|
|fedora|latest|no|
|fedora|rawhide|yes|
|opensuse|latest|no|
|ubuntu|rolling|yes|
|ubuntu|devel|yes|
|ubuntu|latest|no|

This role has been tested on these Ansible versions:

- ansible~=2.8
- ansible~=2.9
- git+https://github.com/ansible/ansible.git@devel

The indicator '\~=' means [compatible with](https://www.python.org/dev/peps/pep-0440/#compatible-release). For example 'ansible\~=2.8' would pick the latest ansible-2.8, for example ansible-2.8.6.

Exceptions
----------

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| EL | No package postgresql-server available |



Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-postgres) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-postgres/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

Modules
-------

This role uses the following modules:
```yaml
---
- command
- lineinfile
- package
- postgresql_user
- service
- template
```

License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/)
