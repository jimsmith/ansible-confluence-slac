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

## Options

There are a couple of booleans that you can use to opt in or out of some sets of tasks. 

**`use_ssl`** when set to true will enable https for the instance (recommended). It will enable tasks that copy over an SSL
keystore that you provide, and adding configuration to `server.xml` to enable https on port 443. A block will also be added
to `web.xml` to automatically redirect http traffic to https. 

**`crowd_auth`** when set to true will configure a crowd.properties file for the instance with the crowd server url and 
app credentials provided in the appropriate vars. 

**`sync_app_data`** is useful if you're using this role to deploy to a testing environment. It will use the given
`sync_server` and `sync_dir` to rsync the confluence home folder from an existing Confluence server. Requires that the
ansible run user exists and has read access to the confluence home folder on the remote server. 

## Using the `additional_files_dir` 

This and other playbooks like it use the convention of an `additional_files_dir` for including non-configuration items
that aren't bundled with this role. For example, I don't think it's appropriate to distribute the jar file for the MySQL
database driver as part of this role. Instead you can download the version of the driver you wish to use, put it in the
folder defined as your `additional_files_dir`, and make sure the filename matches the `msyql_connector_filename` variable. 

Another case where this is used is for `robots.txt`. You may or may not prefer to use a custom `robots.txt` file in your
environment. If you don't, this role will simple leave the bundled default. If you do wish to use a custom robots.txt you
can simply place a file in the additional_files_dir and make sure its name matches the value defined in `robots_filename`.

## Credentials

There are some required variables for various passwords and credentials configured in the app. Those should be stored
in a vault. The following credential vars are required:

    confluence_db_pass: <database_password>
    keystore_pass: <ssl_keystore_password> # Optional (required if use_ssl is true).
    crowd_pass: <crowd_application_password> # Optional (required if crowd_auth is true).
    
    