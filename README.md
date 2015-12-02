# Ansible Role for Confluence

This role will install and setup Confluence using the Standalone Tomcat package and JSVC for RedHat systems. 

Originally written for Confluence deployment at SLAC. 

# Requirements

1. A Redhat server (This role is only tested with RHEL6) with an appropriate Java installation
2. A database (This role assumes MySQL due to the environment it was designed for).

# Variables

As defined in defaults (can be overridden):
    
    # Package prerequisites to install. Our instance uses the LaTeX plugin, so those packages are required.
    rpm_prerequisites:
     - jakarta-commons-daemon-jsvc
     - texlive
     - texlive-latex
     - dvipng
    
    # Confluence app settings
    confluence_version: 5.8.16
    basedir: /opt/j2ee/atlassian
    confluence_home: /var/atlassian/confluence-home
    confluence_user: confluence
    confluence_group: confluence
    download_baseurl: http://www.atlassian.com/software/confluence/downloads/binary
    confluence_installer: atlassian-confluence{{ confluence_version }}.tar.gz
    
    # Database settings (our environment uses
    db_server: mysql.example.com
    db_port: 3306
    db_user: confluence
    db_name: confluence
    
    # If using Crowd for authentication
    crowd_auth: true
    crowd_server: crowd.example.com
    crowd_app: confluence
    
    # SSL Certs
    keystore_dir: /etc/ssl/certs
    keystore_file: confluence.keystore
    
    # Java and Tomcat Options
    # Use the systemwide default jre home set by /sbin/alternatives
    java_home: "/etc/alternatives/jre_oracle"
    mail_disabled: "false" # Set to true to keep staging systems quiet
    min_heap: 2048m
    max_heap: 4096m
    max_perm: 1024m
    tomcat_log: /var/log/tomcat/catalina.out
    tomcat_err_log: /var/log/tomcat/catalina.err
    tomcat_verbose: false
    tomcat_debug: false

## Credentials

There are some required variables for various passwords and credentials configured in the app. Those should be stored
in a vault. The following credential vars are required:

    db_pass: <database_password>
    keystore_pass: <ssl_keystore_password>
    crowd_pass: <crowd_application_password> # Optional (required if crowd_auth is true).
    
    