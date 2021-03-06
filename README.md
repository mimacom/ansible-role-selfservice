# Ansible Role: selfservice

[![Build Status](https://img.shields.io/travis/mimacom/ansible-role-selfservice.svg)](https://travis-ci.org/mimacom/ansible-role-selfservice)

Installs the [self-service-password](https://ltb-project.org/documentation/self-service-password)
web application from the ltb-project. The web application allows users to reset
their password using a SMS token.

This role will install nginx, php, remi in order to work. Configure the
variables to connect to the LDAP server.

Write your own PHP function which connects to your SMS API service of your
choose. Put the PHP file to `/opt/selfservice/reset/lib/smsapi.inc.php`. Call
the PHP function `function send_sms_by_api($mobile, $message)`.

Note that this role will also disable SELinux.

This role is tested on CentOS 7.

## Requirements

You need an LDAP server (Active Directory) and an account at SMSAPI.com

## Role Variables

    # Choose version to install
    selfservice_version: 1.1

    # Set on which vHost the webapp should be available
    server_name: "selfservice.company.invalid"

    # Set LDAP URI
    ldap_url: "ldap://localhost:389"

    # Set Bind user as distinguishedName to connect to the LDAP server
    ldap_binddn: "CN=Administrator,CN=Users,DC=mimacom,DC=local"

    # Set password belonging to bind user
    ldap_bindpw: "secure"

    # Configure search base
    ldap_base: "dc=company,dc=invalid"

    # Used by web application to generate tokens. Set this to a long, random
    # string
    keyphrase: "secure"

    # User mail for smsapi.com
    smsapi_user: "smsapiuser@company.invalid"

    # User API password as md5 hash for smsapi.com
    smsapi_pass: "md5hashedpassword"

    # List of allowed app languages. If empty, all languages are allowed.
    # For possible values, see:
    # https://ltb-project.org/documentation/self-service-password/1.3/config_general
    allowed_lang: []


## Dependencies

These roles will be used:
 * geerlingguy.nginx
 * geerlingguy.php
 * geerlingguy.repo-remi

## Example Playbook

    - hosts: servers
      become: yes
      roles:
        - role: mimacom.selfservice
          selfservice_version: 1.1

## License

Apache License 2.0

## Author Information

This role was created by [Remo Wenger](http://www.remowenger.ch).
