Zeek Systemd
============

Setting up Zeek single-node Zeek cluster using Ansible and systemd. Slight provisions for multi-node, too.

Requirements
------------

* systemd
* Zeek installed from Source or Binary packages.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.


Example Playbook
----------------

    - hosts: servers
      tasks:
      - name: Deploy Zeek
        include_role:
          name: 'zeek-systemd'
        vars:
          zeek_workers: 8
          zeek_interface: af_packet::enp6s0

License
-------

BSD
