## What is ansible-letsencrypt? [![Build Status](https://secure.travis-ci.org/nickjj/ansible-letsencrypt.png)](http://travis-ci.org/nickjj/ansible-letsencrypt)

It is an [Ansible](http://www.ansible.com/home) role to generate and automate
renewal for Let's Encrypt SSL certificates. It uses
[acme-tiny](https://github.com/diafygi/acme-tiny).

##### Supported platforms:

- Ubuntu 16.04 LTS (Xenial)
- Debian 8 (Jessie)

### What problem does it solve and why is it useful?

Let's Encrypt allows you to create free SSL certificates. They have a short
half-life and must be renewed every 90 days or they will expire.

Having to manually keep track of renewals is an excellent way to forget by
accident so this role will do everything for you.

Feel free to use it with any web server configuration you want. If you
want a near zero configuration nginx role to go along with this role then check
out my https://github.com/nickjj/ansible-nginx role.

## Role variables

Below is a list of default values along with a description of what they do.

```
---

# A list of domain names to register certificates for. This has a limit of 100.
# The first item in the list will end up being the file name, for example:
#   letsencrypt_domains: ['example.com', 'www.example.com']
# The above will create example.com.pem and example.com.key files that work for
# both the root domain and the www sub-domain.
letsencrypt_domains: []

# Which web server services need to get restarted at the end of the run?
letsencrypt_restart_services: ['nginx']

# Installation paths.
letsencrypt_install_path: '/usr/local/acme-tiny'
letsencrypt_challenge_path: '/usr/share/nginx/challenges/.well-known/acme-challenge'
letsencrypt_certificate_path: '/etc/nginx/ssl'
letsencrypt_log_path: '/var/log/acme-tiny'

# Generated private keys and certificates.
letsencrypt_account_key_path: '{{ letsencrypt_install_path }}/account.key'
letsencrypt_domain_csr_path: '{{ letsencrypt_install_path }}/{{ letsencrypt_domains[0] }}.csr'
letsencrypt_valid_certificate_path: '{{ letsencrypt_install_path }}/{{ letsencrypt_domains[0] }}.crt'

# Final generated private key and certificate.
letsencrypt_domain_key_path: '{{ letsencrypt_certificate_path }}/{{ letsencrypt_domains[0] }}.key'
letsencrypt_chained_pem_path: '{{ letsencrypt_certificate_path }}/{{ letsencrypt_domains[0] }}.pem'

# Locations of the intermediate certificate used in the chain.
letsencrypt_intermediate_url: 'https://letsencrypt.org/certs/lets-encrypt-x3-cross-signed.pem'

# By default we'll use the staging server endpoint so that you're forced to
# explicitly set this when you're ready for production.
#
# For production, set this to:
#   letsencrypt_default_ca: 'https://acme-v01.api.letsencrypt.org'
letsencrypt_default_ca: 'https://acme-staging.api.letsencrypt.org'

# How often should we try to renew certificates? Default is once per month.
letsencrypt_cron_renew: ['0', '0', '1', '*', '*']

# When set to True this will delete all private keys, CSRs and certificates.
# You would use this when you want to force create new certificates, such as
# adding a new sub-domain to your certificate file.
#
# Don't always keep this enabled or you'll likely run into rate limits.
letsencrypt_force_reset_all: False
```

## Example playbook

For the sake of this example let's assume you have a group called **app** and
you have a typical `site.yml` file.

To use this role edit your `site.yml` file to look something like this:

```
---

- name: Configure app server(s)
  hosts: app
  become: True

  roles:
    - { role: nickjj.letsencrypt, tags: letsencrypt }
```

Let's say you want to generate certs for blog.example.com in production.
You can do this by opening or creating `group_vars/app.yml` which is located
relative to your `inventory` directory and then making it look like this:

```
---

letsencrypt_domains: ['blog.example.com']
letsencrypt_default_ca: 'https://acme-v01.api.letsencrypt.org'
```

## Revoking certificates

Certificates expire in 90 days and you can issue new certificates at will so
there's no way to revoke them built into this role.

If you really need to revoke them then I recommend checking out
[this project](https://github.com/diafygi/letsencrypt-nosudo) because it
includes a way to revoke certificates and is made by the same author as acme-tiny.

## Installation

`$ ansible-galaxy install nickjj.letsencrypt`

## Ansible Galaxy

You can find it on the official
[Ansible Galaxy](https://galaxy.ansible.com/nickjj/letsencrypt/) if you want to
rate it.

## License

MIT

## Special thanks

Thanks to [Maciej Delmanowski](https://twitter.com/drybjed) for helping me debug
a few tricky issues with this role. He is the creator of [DebOps](https://debops.org/).
