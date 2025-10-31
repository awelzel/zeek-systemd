This repository was archived. Since release 8.1, Zeek ship a [zeek-systemd-generator](https://github.com/zeek/zeek/tree/master/tools/systemd-generator) that will setup a Zeek cluster based on the `zeek.conf` configuration file.

---


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

Differences from Zeekctl
------------------------

For those familiar with zeekctl and it's configuration, there are a couple important things to note.  The node.cfg and networks.cfg files are not used with a systemd configuration.

Previously, zeekctl would convert node.cfg file into a cluster-layout.zeek file that would be placed into your local site directory when zeek was deployed and automatically loaded when zeek starts.  The provided ansible tasks create this for you instead.

Another important file was networks.cfg where you would define your local networks.  This was converted by zeekctl into local-networks.zeek that has the following format:

```
redef Site::local_nets = {
        10.0.0.0/24,        # Some local ipv4 space
        [fe80::]/120,       # Some local ipv6 space
};
```

The current interation of this repo does _not_ create this file for you.

Use and Debugging
---------

With this setup you can start/stop zeek using systemctl:

```
systemctl [start/stop/restart] zeek.target
```
Since we are using systemctl, we can also use various forms of journalctl to do some debugging:
```
journalctl -u 'zeek-worker@1' -n 20         # get data about a specific worker
journalctl -u 'zeek-worker*' -n 20          # get data about all workers
journalctl -u 'zeek-manager@logger' -n 20   # get data about the manager/proxy/logger
journalctl -u 'zeek*' -n 20                # get everything
journalctl -u 'zeek*' -n 20 --no-pager     # no-pager for long lines
```

Often times if there is a script error it will be necessary to try running zeek by hand to find the exact issue.  This configuration creates an ansible-local directory which you may also want to include when test loading zeek:

```
export CLUSTER_NODE=manager
zeek -a ansible-local

export CLUSTER_NODE=worker
zeek -i $INTERFACE ansible-local
```

License
-------

BSD
