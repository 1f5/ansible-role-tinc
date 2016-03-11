Ansible Role for Tinc
=====================

Ansible role to install, configure, and use tinc.

Requirements
------------

* Developed and tested with Ansible 2.0.
* Debian/Ubuntu system.

Role Variables
--------------

* **mode** - *string* (default is **default**)

  * **default**
    
    Installs tinc if needed, and makes sure the given network is configured.

  * **push**
    
    Redistribute cached host configs to all nodes.

  * **remove**

    Remove configuration for a given network.

  * **uninstall**

    Remove network configurations and uninstall tinc.

  * **start**

    Starts tinc service. (persistent across reboots)

  * **restart**

    Restarts tinc service.

  * **stop**

    Stop tinc service.

  * **disable**

    Stop tinc service and disable it. (persistent across reboots)


* **regen_keys** - *boolean* (default is **no**)

    Force regeneration of RSA key pair when running default mode on an existing setup.   

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

  Virtual network node IP address. Override for each node.

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


Other Variables
---------------

* **netgroup** - *string* (defaults to netname)

  Name of group in inventory containing all nodes for given network.

Dependencies
------------

From apt module:
* python-apt
* aptitude

Example Playbook
-------------------------

A tinc network is defined in the inventory as follows:

    [foovpn] # where foovpn is the tinc network name
    node1 node_ip=10.0.1.1
    node2 node_ip=10.0.1.2
    node3 node_ip=10.0.1.3


Here is an example playbook that installs tinc, configures a network and starts it.

    ---
    # Make sure tinc is installed and foovpn network configured
    - hosts: foovpn
      vars:
      - netname: "foovpn"
      roles:
      - tinc


Another example specifying the operation mode:

    ---
    # Redistribute host configurations and restart tinc
    - hosts: foovpn
      vars:
      - netname: "foovpn"
      roles:
      - { role: tinc, mode: push }


Caveat: Make sure that host_ip is correctly set. In a default NAT vagrant setup, each node will have the same IP for eth0.


License
-------

GPLv3 - see LICENSE file for details

Author Information
------------------

You can contact me at [e zncb.io](mailto:ey%40zncb.io);

