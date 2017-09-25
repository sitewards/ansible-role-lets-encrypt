# Ansible Lets Encrypt role

This is the Ansible lets-encrypt role. It's designed for consumption by playbooks, not for consumption by
itself.

## Requirements

- Internet Access
- Ansible 2.4.0+
- Python2[1](https://github.com/ansible/ansible/issues/30690)
- pip (installs dependencies if required)

## Terminology

| Word      | Meaning                                                                                                          |
|-----------|------------------------------------------------------------------------------------------------------------------|
| provider  | A mechanism that provides proof that the server is owned by the same tooling that is requesting the certificate  |
| mechanism | The Lets Encrypt protocol used for verifying server authentication                                               |
| type      | The type of provider that will be implementing the required authorisation                                        |

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

```yaml
# $PLAYBOOK_ROOT/server.yaml

---
- name: "server"
  hosts: all
  become: true
  become_user: "root"
```

Add the reference for the role:

```yaml
# $PLAYBOOK_ROOT/server.yaml

# ...
become_user: "root"
roles
  - "sitewards.lets-encrypt"
```

This will allow the role to be discovered. Then, add this repo as a submodule:

```bash
    $ cd path/to/playbook/root
    $ mkdir roles/
    $ git submodule add https://github.com/sitewards/ansible-role-lets-encrypt roles/sitewards.lets-encrypt
```

This should work!

## Design Notes:

### Non goal: Allow multiple certificates

This role deliberately does not factor in the usage of multiple certificates. This can be accomplished by using the role
multiple times, with different variables. [3](https://stackoverflow.com/questions/32802956/ansible-running-role-multiple-times-with-different-parameter-sets)

## Configuration

The variables that are available are defined in defaults/main.yml. There are various requirements; check the file for
further details.

## Debugging

The task can be skipped by not defining a "lets_encrypt_common_name" variable (undefined by default). This is configured
so that the role can be provisioned across all environments (including internally, where there is no mechanism to
validate lets encrypt).

Should the task be skipping, take a look at that.

## Development

### Yaml files

Because files are dynamically included, the suffix ".yml" *must* be used for all yaml files.

### Comments

Comments starting with:

```yaml
# foo: "bar"
```

Are examples. Comments starting with:

```yaml
## This is example text.
```

are documentation

## Contact

https://www.sitewards.com/
