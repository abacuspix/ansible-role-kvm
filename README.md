ansible-role: kvm
=========

Install KVM and manage vms and vbridges.
If wished it can setup an internal HTTP server for serving pressed files.

This role is pretty basic. It currently supports only Debian Jessie.
Storage is hardcoded for use with LVM.

Each project should get an own vbridge. The identifier of a vbridge is the domain name of a vm. For example a vm with with domain ```example.org``` requieres a vbridge named ```example.org```.

Requirements
------------

None

Role Variables
--------------

List of variables, their defaults and what they are used for.

Install virt-manager and his dependencies

    kvm_virt_manager: yes

Install Nginx to server preseed files

    kvm_preseed_server: yes

DocumentRoot where preseed files should be stored

    kvm_preseed_docroot: /var/www/preseed

List of networks Nginx should bind on. ```auto``` will discover all vbridges managed by libvirt. To bind only on specific interfaces use a list of interfaces instead

    kvm_preseed_nics: auto

Mirror which should be used for future updates

    kvm_preseed_cfg_mirror: http://ftp2.de.debian.org/

Directory of the mirror

    kvm_preseed_cfg_mirror_directory: debian

Passwort hash of the root user

    kvm_preseed_cfg_root_password: false

Timezone of the machine

    kvm_preseed_cfg_timezone: Europe/Berlin

Public key which should be injected into the vm

    kvm_preseed_cfg_public_key: false

Dictionary with vbridges that can be spinned up (will be refactored on Ansible 2.0 release)

    kvm_bridges:
      default:
        nic: virbr0
        forward: true
        ip: 192.168.122.1
        netmask: 255.255.255.0
        dhcp: true
        start: 192.168.122.2
        end: 192.168.122.254

Location which is used for bootstraping a vm

    kvm_vm_manage_location: http://ftp2.de.debian.org/debian/dists/{{ item.0.os }}/main/installer-amd64/

List whit vms which should be available. Note that this will only create the vms and bootstrap them. Anything else like changeing number of CPUs, RAM, etc is not supported.

    kvm_vm_manage: []

Dependencies
------------

None

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

playbook:
    - hosts: servers
      roles:
         - kvm


host_vars:
    kvm_bridges:
      my-domain.org:
        nic: virbr0
        forward: true
        ip: 10.100.0.1
        netmask: 255.255.255.0
    kvm_vm_manage:
      - name: lb
        domain: my-domain.org
        vcpu: 2
        ram: 2048
        disk: 25
        os: jessie
        ip: 10.100.0.10
      - name: app
        domain: my-domain.org
        vcpu: 8
        ram: 4096
        disk: 100
        os: jessie
        ip: 10.100.020

License
-------

CC-BY
