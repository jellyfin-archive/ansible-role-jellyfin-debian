# Ansible Role: Jellyfin

[![Build Status](https://travis-ci.com/TiedToAStar/ansible-role-jellyfin-debian.svg?branch=master)](https://travis-ci.com/TiedToAStar/ansible-role-jellyfin-debian)

An Ansible Role that installs [Jellyfin](https://github.com/jellyfin/jellyfin) on Debian and Ubuntu.

## Requirements

None.

## Role Variables

* `jellyfin_HTTP_port`: HTTP port Jellyfin should bind to, defaults to `8096`
* `jellyfin_HTTPS_port`: HTTPS port Jellyfin should bind to, defaults to `8920`
* `jellyfin_FQDN`: List of Fully Qualified Domain Names of the server
* `jellyfin_HTTP_server`: HTTP reverse proxy server, possible values are
  `apache2` and `nginx`, defaults to `nginx`
* `jellyfin_certificate_path`: directory where the TSL/SSL certificate will be
* `jellyfin_certificate_via_letsencrypt_certbot`: whether to have [autocert bot](https://certbot.eff.org/) handle the certs, defaults to `true`. If you set this to `false`, make sure to have
  a certificate and private key in `jellyfin_certificate_path`, otherwise the
  role will fail to start the reverse proxy HTTP server. 
* `jellyfin_remove_default`: set this to `true` to remove apache2/nginx default
  site
* `jellyfin_redirect_HTTPS`: set this to `true` to have the reverse proxy
  automatically redirect requests to HTTPS, defaults to `false`

# Dependencies

* [geerlingguy.certbot](https://github.com/geerlingguy/ansible-role-certbot)
* [tiedtoastar.openldap-debian](https://github.com/tiedtoastar/ansible-role-openldap-debian)

# Example Playbook

```yaml
- name: Install ldap & Jellyfin
  hosts: all
  become: true
  roles:
    - role: tiedtoastar.openldap-debian
      openldap_is_setup: false
      openldap_FQDN: www.example.com
      openldap_domain: dc=example,dc=com
      openldap_dc: example
      openldap_o: example com
      openldap_rootpw: ChangeMe1
      openldap_user: ansible
      openldap_group: ansible
      openldap_groups:
      - jellyfinusers:
        attributes:
          description: Jellyfin users
          member: uid=jellyfinadmin,ou=people,{{ openldap_domain }}
      - webadmins:
        attributes:
          description: admins that are both machines accounts and people
          member: uid=jellyfinadmin,ou=people,{{ openldap_domain }}
      openldap_users:
      - dn: uid=jellyfin-ldap-service,ou=machineaccounts,{{ openldap_domain }}
        objectClass:
        - inetOrgPerson
        - simpleSecurityObject
        userPassword: ChangeMe2
        attributes:
          cn: jellyfin-ldap-service
          sn: jellyfin-ldap-service
          description: accounts used to connect jellyfin and the ldap for user authentication
      - dn: uid=jellyfinadmin,ou=people,{{ openldap_domain }}
        objectClass:
        - inetOrgPerson
        userPassword: ChangeMe3
        attributes:
          cn: jellyfinadmin
          sn: jellyfinadmin
          description: account used to administrate the jellyfin web application
    - role: tiedtoastar.jellyfin-debian
      jellyfin_FQDN: "{{ openldap_FQDN }}"
      jellyfin_plugins:
        ldap:
          ldapbasedn: ou=people,{{ openldap_domain }}
          ldapsearchfilter: (memberOf=cn=jellyfinusers,ou=groups,{{ openldap_domain }})
          ldapadminfilter: (memberOf=cn=jellyfinusers,ou=groups,{{ openldap_domain }})(memberOf=cn=webadmins,ou=groups,{{ openldap_domain }})
          ldapbinduser: uid=jellyfin-ldap-service,ou=machineaccounts,{{ openldap_domain }}
          ldapbindpassword: ChangeMe2

```

## License

MIT

## Author Information

TiedToAStar

## Want to help?

If you find this project useful, please consider a donation to the following Monero address: 47q3TVnd79QcMLqFE2HJC5HTWDadUXtMDVavERPfeT3xFiBeqQQX6knBNALTz4aciC6pSbnLoMCHXXsQDCPV1BT7TqoqZxW

Haven't heard of Monero? Then do me the favor by checking out: https://www.getmonero.org/
