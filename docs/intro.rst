Introduction
------------

Plone's Ansible Playbook can completely provision a remote server to run a full-stack, production-ready Plone server, including:

* Plone in a cluster configuration;

* Automatic starting and process control of the Plone cluster with `supervisor <http://supervisord.org>`_;

* Load balancing of the cluster with `HAProxy <http://www.haproxy.org/>`_;

* Caching with `Varnish <https://www.varnish-cache.org/>`_;

* `Nginx <http://wiki.nginx.org/Main>`_ as a world-facing reverse proxy and URL rewrite engine;

* An outgoing-mail-only mail server using `Postfix <http://www.postfix.org/>`_;

* Monitoring and log analysis with `munin-node <http://munin-monitoring.org/>`_, `logwatch <http://linuxcommand.org/man_pages/logwatch8.html>`_ and `fail2ban <http://www.fail2ban.org/wiki/index.php/Main_Page>`_.

* Use of a local `VirtualBox <https://www.virtualbox.org/>`_ provisioned via `Vagrant <https://www.vagrantup.com/>`_ to test and model your remote server.

An Ansible playbook and roles describe the desired condition of the server. The playbook is used both for initial provisioning and for updating.

.. note ::

    If you want to take more control of your playbook, the `Plone server role <https://github.com/plone/ansible.plone_server>`_ is available by itself, and is listed on `Ansible Galaxy <https://galaxy.ansible.com/list#/roles/2212>`_.

TL;DR
^^^^^

1. Install a current version of Ansible;

2. If you wish to test locally, install Vagrant and VirtualBox;

3. Check out or download a copy of this package;

4. Run ``ansible-galaxy -p roles -r requirements.txt install`` to install required roles;

5. Edit ``configure.yml`` to override settings; create and edit ``local-configure.yml`` if you don't wish to change parts of the distribution;

6. To test in a local virtual machine, run ``vagrant up`` or ``vagrant provision``;

7. To deploy, create an Ansible inventory file for the remote host and run ``ansible-playbook --ask-sudo-pass -i myhost playbook.yml``;

8. Set a real password for your Plone instance on the target server;

9. Set up appropriate firewalls.


Automated-server provisioning
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The goal of an automated-server provisioning system like Ansible is a completely reproducible server configuration. If you wish to achieve this goal, discipline yourself to never changing configuration on your target machines via login.

That doesn't mean you never log in to your provisioned server. It just means that when you do, you resist changing configuration options directly. Instead, change your playbook, test your changes against a test server, then use your playbook to update the target server.

We chose Ansible for our provisioning tool because:

1) It requires no client component on the remote machine. Everything is done via ssh.

2) It's YAML configuration files use structure and syntax that will be familiar to Python programmers. YAML basically represents a Python data structure in an outline. Conditional and loop expressions are in Python. Templating via Jinja2 is simple and clean.

3) `Ansible's documentation <http://docs.ansible.com>`_ is excellent and complete.

4) Ansible is easily extended by roles. Many basic roles are available on `Ansible Galaxy <http://galaxy.ansible.com>`_.
