Tinc Ansible Role
==================

Ansible role to install, configure, and use tinc.

Requirements
------------

* Developed and tested with Ansible 2.0.
* Debian/Ubuntu system.

Role Variables
--------------

* **mode** - *string* (default is **sync**)

  * **install**
    
    Installs and initializes a tinc network.

  * **update**
    
    Regenerate config files and update local host cache. (keys untouched)

  * **sync**
    
    Redistribute host configs to all nodes.

  * **uninstall**

    Remove configurations for a given tinc network.

  * **start**

    Starts tinc service. (persistent across reboots)

  * **restart**

    Restarts tinc service.

  * **stop**

    Stop tinc service.

  * **disable**

    Stop tinc service and disable it. (persistent across reboots)

Role Defaults
-------------

* **netname** - *string* (default is **tincvpn**)

  Name of the network concerned by this run.

* **addrfam** - *string* (default is **ipv4**)

  Address family, anything other than ipv4 is not supported yet. (future)

* **iface** - *string* (default is **tun0**)

  Interface to use for the network.

* **host_ip** - *string* (default is **{{ansible_eth0.ipv4.address}}**)

  Host-side IP address.

* **node_ip** - *string* (default is **10.0.0.1**)

  Virtual network node IP address within network. Override for each node.

* **netmask** - *string* (default is **255.255.255.0**)

  Virtual network mask.

* **keysize** - *string* (default is **4096**)

  RSA key size.

* **compression** - *string* (default is **11**)

  Tinc compression level.

* **cipher** - *string* (default is **aes-256-gcm**)

  Tinc encryption cipher.

* **digest** - *string* (default is **sha384**)

  Tinc authentication digest.

* **host_cache** - *string* (default is **.tinc_hosts**)

  Local cache for host config files.

Dependencies
------------

None.


Example Playbook
-------------------------

A tinc network is defined in the inventory as follows:

    [tinc_foovpn] # where foovpn is the tinc network name
    node1 node_ip=10.0.1.1
    node2 node_ip=10.0.1.2
    node3 node_ip=10.0.1.3

Here is an example playbook that installs tinc, configures a network and starts it.

    ---
    # Install tinc and configure foovpn network
    - hosts: tinc_foovpn
      vars:
      - netname: "foovpn"
      roles:
      - { role: tinc, mode: install }
    
    # Sync all hosts and start vpn
    - hosts: tinc_foovpn
      vars:
      - netname: "foovpn"
      roles:
      - { role: tinc, mode: sync }


Caveat: Make sure that host_ip is correctly set. In a default NAT vagrant setup, each node will have the same IP for eth0.


License
-------

GPLv3 - see LICENSE file for details

Author Information
------------------

You can contact me at [e zncb.io](mailto:ey%40zncb.io);

