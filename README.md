# SLAC Confluence for Ansible

This role will install and setup Confluence using the Standalone Tomcat package and JSVC for RedHat systems. 

Tailored specifically for Confluence deployment at SLAC. Can probably be retooled for other environments.

# Requirements

1. A RedHat Linux server (This role is only tested with RHEL6) with Java installed.
2. A database (This role assumes MySQL due to the environment it was designed for).

# Variables

All of the required variable save for credentials (which you should put in a vault) are defined in `defaults/main.yml`.

These can be overridden locally. In fact there are several that probably should be since I've just given them dummy 
values as placeholders. Make sure to examine them carefully before running this against your systems. 

There are a couple of booleans that you can use to opt in or out of some sets of tasks. 

`use_ssl` when set to true will 
## Credentials

There are some required variables for various passwords and credentials configured in the app. Those should be stored
in a vault. The following credential vars are required:

    db_pass: <database_password>
    keystore_pass: <ssl_keystore_password>
    crowd_pass: <crowd_application_password> # Optional (required if crowd_auth is true).
    
    