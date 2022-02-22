Ansible Role: Third Party Apt Repo
================================================================================

An ansible role that will add a third party apt repo to and Debian like distro.
It can optionally install packages after adding the repo.

This role has only been tested on Ubuntu 20.04.

Role Variables
----------------------------------------

A string in apt source list format. This string will be passed to
`ansible.builtin.apt_repository.repo`.
```yml
third_party_repo: deb [arch=amd64] https://apt.releases.hashicorp.com focal main
```

A URL to the key that signed packages from the 3rd party repo. This string will
be passed to `ansible.builtin.apt_key.url`.
```yml
third_party_repo_key_url: https://apt.releases.hashicorp.com/gpg
```

The fingerprint of the key pointed to by `third_party_repo_key_url`. This string
will be passed to `ansible.builtin.apt_key.id`. See section below on how to find
this key.
```yml
third_party_repo_key_fingerprint: E8A032E094D8EB4EA189D270DA418C88A3219F7B
```

A list of packages to install after the third party repo has been added. These
can be packages from the third party repo or from the default repos.
```yml
packages: [terraform]
```

Install Role
----------------------------------------

Create a yaml file with the following content.
```yml
---
- src: "git+https://gitlab.mss.com/ANDSAS/ops/ansible/\
        third_party_apt_repo_ansible_role.git"
  name: third_party_apt_repo
  version: master
```

Then run:
```shell
ansible-galaxy install -r <requirement yaml file>
```

Example Playbook
----------------------------------------

```yml
- roles:
    - role: install_via_3rd_party_apt_repo
      third_party_repo: |
        deb [arch=amd64] https://apt.releases.hashicorp.com focal main
      third_party_repo_key_url: https://apt.releases.hashicorp.com/gpg
      third_party_repo_key_fingerprint: E8A032E094D8EB4EA189D270DA418C88A3219F7B
      packages: [terraform]
```

GPG Key Fingerprint
----------------------------------------

Here is a way to get a fingerprint for a key via gpg. This method will not
import the key. The command uses the `-n` flag which tells gpg that this is a
dry run and to not import the key.

```shell
$ > gpg2 -n -q --import --import-options import-show <gpg key>
pub   rsa4096 2020-05-07 [SC]
      E8A032E094D8EB4EA189D270DA418C88A3219F7B
uid                      HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>
sub   rsa4096 2020-05-07 [E]
```

In this example the hex string `E8A032E094D8EB4EA189D270DA418C88A3219F7B` is the
fingerprint.

License
----------------------------------------

MIT

Author Information
----------------------------------------

This role was created by [shnee](https://github.com/shnee).
