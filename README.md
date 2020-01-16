# Ansible Role: Jellyfin

[![Build Status](https://travis-ci.com/TiedToAStar/ansible-role-jellyfin-debian.svg?branch=master)](https://travis-ci.com/TiedToAStar/ansible-role-jellyfin-debian)

An Ansible Role that installs [Jellyfin](https://github.com/jellyfin/jellyfin) on Debian and Ubuntu.

<img src="https://media.giphy.com/media/L3o1VDj2ic7DnDTvII/source.gif"/>

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
See example
If you only want jellyfin installed, remove two roles: tiedtoastar.pwm & tiedtoastar.openldap

```

## License

MIT

## Author Information

TiedToAStar

## Want to help?

If you find this project useful, please consider a donation to the following Monero address: 47q3TVnd79QcMLqFE2HJC5HTWDadUXtMDVavERPfeT3xFiBeqQQX6knBNALTz4aciC6pSbnLoMCHXXsQDCPV1BT7TqoqZxW

Haven't heard of Monero? Then do me the favor by checking out: https://www.getmonero.org/
