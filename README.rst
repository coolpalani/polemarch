Polemarch
=========

**Polemarch** is service for orchestration infrastructure by ansible.
Simply WEB gui for orchestration infrastructure by ansible playbooks.

Official site: https://gitlab.com/vstconsulting/polemarch

.. image:: https://raw.githubusercontent.com/vstconsulting/polemarch/master/doc/screencast.gif
   :alt: interface of Polemarch
   :width: 100%

Features
--------

* scheduled tasks execution;
* share hosts, groups between projects;
* history of tasks execution with all details;
* easy configurable clustering for reliability and scalability:
* import Ansible projects from Git repository or tar archive;
* documentation: http://polemarch.readthedocs.io/en/latest/ ;
* groups of hosts and groups hierarchy;
* multiuser;
* responsive design;

Red Hat/CentOS installation
---------------------------

1. Download rpm from official site:
   https://github.com/vstconsulting/polemarch/releases

2. Install it with command

   .. sourcecode:: bash

      sudo yum localinstall polemarch-0.0.2-0.x86_64.rpm.

3. Run services with commands

   .. sourcecode:: bash

      sudo service polemarchweb start
      sudo service polemarchworker start

That's it. Polemarch web panel on 8080 port. Default administrative account is
admin/admin.

Note: If you using authentication by password at some of your machines
managed by Polemarch, you also must install ``sshpass`` package because it
required for ansible to autheticate via ssh by password. It available in
EPEL for Red Hat/CentOS. Also you can use specify ``connection`` command line
argument during playbook run as ``paramiko``. When ansible uses paramiko to
make ssh connection, ``sshpass`` not necessary.

Ubuntu/Debian installation
--------------------------

1. Download deb from official site:
   https://github.com/vstconsulting/polemarch/releases

2. Install it with command

   .. sourcecode:: bash

      sudo dpkg -i polemarch_0.0.2-1_amd64.deb || sudo apt-get install -f

3. Run services with commands

   .. sourcecode:: bash

      sudo service polemarchweb start
      sudo service polemarchworker start

That's it. Polemarch web panel on 8080 port. Default administrative account is
admin/admin.

Quickstart
----------

After you install Polemarch by instructions above you can use it without any
further configurations. Interface is pretty intuitive and common for any web
application.

Default installation is suitable for most simple and common cases, but
Polemarch is highly configurable system. If you need something more advanced
(scalability, dedicated DB, custom cache, logging or directories) you can
always configure Polemarch like said in documentation at
http://polemarch.readthedocs.io/en/latest/

How to participate
------------------

If you found some bug in the codebase or documentation or want some
improvement in project, you can easily do it myself. Here is how to proceed:

1. Create (or find if already exist) an issue at
https://gitlab.com/vstconsulting/polemarch for problem you want to solve. Maybe
somebody is working on that or maybe during discussion will become clear that
is not a problem.

2. Make your fork of our repository and clone it to your local development
machine.

   .. sourcecode:: bash

      git clone https://gitlab.com/cepreu/polemarch.git

3. Create a branch for your work named with number of issue:

   .. sourcecode:: bash

      cd polemarch
      git checkout -b issue_1

4. To investigate problem, you probably must be able to run cloned project.
To do so install required dependencies. See
http://polemarch.readthedocs.io/en/latest/pipinstall.html for more detailed
explanation, which dependencies and why they are needed. System packages
(example for Ubuntu 16.04):

   .. sourcecode:: bash

      sudo apt-get install apache2 apache2-dev python-pip python-dev libffi-dev libssl-dev git sshpass

Python dependencies:

   .. sourcecode:: bash

      pip install -r requirements-git.txt -r requirements.txt -r requirements-doc.txt tox --user

5. Initialize empty database with all required stuff (tables and so on)
   for Polemarch:

   .. sourcecode:: bash

      ./polemarchctl migrate

6. Run Polemarch and investigate with your debugger how it works to find out
   what need to be changed:

   .. sourcecode:: bash

      # run web-server
      ./polemarchctl webserver -P 8080

   .. sourcecode:: bash

      # run worker
      ~/.local/bin/celery -A polemarch.celery_app:app worker -l INFO -B -S django

7. You may also want to change ``./polemarch/main/settings.ini`` to enable
   ``debug`` setting and to change ``log_level`` for easy debugging.

8. Write tests for your changes and changes itself (we prefer TDD approach).
   Execute those tests with all other Polemarch's tests by:

   .. sourcecode:: bash

      make test

   This command also doing PEP8 check of codebase and static analyzing with
   pylint and flake. Make sure that your code meet those checks.

9. Reflect your changes in documentation (if needed). Build documentation, read
   what you changed and make sure that all right. To build documentation use:

   .. sourcecode:: bash

      make docs

10. Make commit. We prefer commit messages with briefly explains your
    changes. Bad: "issue #1" or "fix". Good: "fix end slashes for GET in docs".

11. Create pull request and refer it in issue.

**ATTENTION**: Keep in mind, that we expect that all your changes are
MIT-licensed to prevent any license problems to project in future.

That's it. Thank you for your contribution.