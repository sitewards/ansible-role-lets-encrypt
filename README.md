# Ansible Lets Encrypt role

This is the Ansible lets-encrypt role. It's designed for consumption by playbooks, not for consumption by
itself.

## Requirements

- Internet Access
- Ansible 2.4.0+
- Python2[1](https://github.com/ansible/ansible/issues/30690)
- pip (installs dependencies if required)

## Caveats

### Limited Support

While the role was written in an extensible way, and wll be extended as requirements dicatate to include other
Lets Encrypt auth mechanisms or cloud providers, only DNS by Route53 has been implemented so far.

### Manually combines full chain

At this time, it is not possible to fetch the full chain from Ansible[3](https://github.com/ansible/ansible/pull/22074).
Given this, the current certificate chain is manually constructed from what's available on Lets Encrypt publicly.
This may go out of date or otherwise break.

### Only DNS SANs are supported

This is a limitation of Lets Encrypt upstream[2](https://community.letsencrypt.org/t/register-ip-as-san-for-my-domain/12703)
Given this, the limitation is also part of the role. Should this limitation be revised in future, this role will need
to be refactored.

### Does not install self renewing software

This does not install something like certbot etc. to self renew software. It does renew certificates early (30 days by
default), but the role of Ansible is to run machines including PKI / expiring certificates.

## Usage

Include this in another ansible playbook. For sample, consider a generic server playbook:

```
---
# $PLAYBOOK_ROOT/server.yaml
- name: "server"
  hosts: all
  become: true
  become_user: "root"
```

Add the reference for the role:

```
# $PLAYBOOK_ROOT/server.yaml
# ...
become_user: "root"
roles
  - "sitewards.lets-encrypt"
```

This will allow the role to be discovered. Then, add this repo as a submodule:

```
$ cd path/to/playbook/root
$ mkdir roles/
$ git submodule add https://github.com/sitewards/ansible-role-lets-encrypt roles/sitewards.lets-encrypt
```

This should work!

## Configuration

The variables that are available are defined in defaults/main.yml. There are various requirements; check the file for
further details.

## Debugging

The task can be skipped by not defining a "lets_encrypt_common_name" variable (undefined by default). This is configured
so that the role can be provisioned across all environments (including internally, where there is no mechanism to
validate lets encrypt).

Should the task be skipping, take a look at that.

## Contact

https://www.sitewards.com/
