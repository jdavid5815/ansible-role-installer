# Role Name

The "installer" role allows you to install packages (and optionally start services) for Redhat and Debian based OS distributions, by using a single package name. This single name is defined in the defaults and thus avoids having to remember package and service names for different distributions.

## Requirements

There a no specific requirements. If the package you want to install is not in the defaults, then you will need to add the necessary parameters once.

## Role Variables

There is exactly one variable that must be specified in your playbook: 'packages'. This variable must contain at least one hash, specifying a package name and a boolean which is used to determine if a service needs to be enabled and started (true, if not: false). Example:

`vars:`
  `packages:`
    `docker: true`

The key of the hash value (docker in the example above), must also be defined in the "defaults/main.yml" yaml file, where it is used to map the "chosen" package name, to the actual, os-dependant package and service name.

Example:

mappings:
  docker:
    package: "{% if ansible_os_family == 'RedHat' %}docker{% elif ansible_os_family=='Debian' %}docker.io{% else %}docker{% endif %}"
    service: "{% if ansible_os_family == 'RedHat' %}docker{% elif ansible_os_family=='Debian' %}docker{% else %}docker{% endif %}"

## Dependencies

None.

## Example Playbook

---
- hosts: all

  become: yes

  vars:
    packages:
      docker: true
      ntp: true
      psmisc: false

  roles:
    - installer
...

## License

BSD

## Author Information

Created by Jan David, DevOps at Evolane N.V.
