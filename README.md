ipaserver
=========

A parameterized role for setting up a FreeIPA server as a primary or replica. Primarily tested on CentOS 7 and CentOS 8.

Requirements
------------

Should work on any Red Hat 7.4+ clone.

Role Variables
--------------

There are 2 main variables that need to be provided external to the role that have no default. 

    ipaserver_admin_password
    ipaserver_dir_admin_password

The following variables are used by this role and values are defined in defaults/main.yml:

    ipaserver_dns_forwarder: 8.8.8.8
    ipaserver_domain: example.com       # All lowercase. Actual DNS domain.
    ipaserver_realm: EXAMPLE.COM        # All caps. Better to match domain, but not required
    ipaserver_setup_dns: True
    ipaserver_setup_ntp: True
    ipaserver_primary: 1 # set to 1 for primary. Anything else to set to replica.



Example Playbook
----------------

Here is an example playbook that can readily wrap this role and still be fairly flexible.  You typically don't need to be this flexible on password source.

    - hosts: servers
      vars_files:
        - vars/private-idm.yml
      vars_prompt:
        - name: ipaserver_admin_password
          prompt: "What should the admin password be for IPA?"
          private: yes
          default: "{{ vault_ipaserver_admin_password }}"
        - name: ipaserver_dir_admin_password
          prompt: "What should the admin password be for the Directory Server?"
          private: yes
          default: "{{ vault_ipaserver_dir_admin_password }}"
      roles:
         - { role: clusterapps.freeipa-server }

License
-------

GPLv2

Author Information
------------------
Michael Cleary <mcleary@clusterapps.com>

Forked from https://github.com/gregswift/ansible-freeipa to add replica servers and more functionality. 
