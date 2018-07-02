# Ansible Role: Environment Modules

[![Build Status](https://travis-ci.org/jkglasbrenner/ansible-role-environment-modules.svg?branch=master)](https://travis-ci.org/jkglasbrenner/ansible-role-environment-modules)

Compiles and installs [environment modules](http://modules.sourceforge.net/) from source on Debian-based and RedHat-based Linux distributions. Environment modules are used for dynamic modification of a user's environment via modulefiles.

Install this role using `ansible-galaxy`:

```bash
ansible-galaxy install jkglasbrenner.environment_modules
```

## Requirements

None.

## Role Variables

Variables and default values:

```yaml
# main miniconda download server
modules_mirror: "https://github.com/cea-hpc/modules/releases/download"

# modules version
modules_version: 4.1.3

# modules checksums
modules_checksums:
  modules-4.1.2.tar.gz: "sha256:d1f54f639d1946aa1d7ae8ae03752f8ac464a879c14bc35e63b6a87b8a0b7522"
  modules-4.1.3.tar.gz: "sha256:dab82c5bc20ccea284b042d6af4bd6eaba95f4eaadd495a75413115d33a3151f"

# modules source code info
modules_name: "modules-{{ modules_version }}"
modules_src_tar_gz: "{{ modules_name }}.tar.gz"
modules_src_url: "{{ modules_mirror }}/v{{ modules_version }}/{{ modules_src_tar_gz }}"
modules_checksum: "{{ modules_checksums[modules_src_tar_gz] }}"

# when downloading the source code it might take a while
modules_timeout_seconds: 300

# make command (use "make -j4" to enable parallel compilation, for example)
modules_make_cmd: make

# where to install modules
modules_parent_dir: /usr/local
modules_etc_profile: /etc/profile.d
modules_dir: "{{ modules_parent_dir }}/Modules"

# locations of init files
modules_init_profile: "{{ modules_dir }}/init/profile"
modules_init_bash: "{{ modules_dir }}/init/bash"
modules_init_modulerc: "{{ modules_dir }}/init/modulerc"
modules_init_use_own: "{{ modules_dir }}/init/use.own"

# locations of modulefiles
modules_modulefiles_dir: "{{ modules_dir }}/modulefiles"
modules_use_own_dir: privatemodules

# example modulefiles to remove
modules_example_modulefiles_to_remove: []
```

Distribution-specific variables:

```yaml
# Debian-based distros
modules_package_dependencies:
  - autoconf
  - automake
  - autopoint
  - bash
  - build-essential
  - coreutils
  - dejagnu
  - grep
  - less
  - sed
  - tcl-dev

# RedHat-based distros: CentOS 7 and Fedora
modules_package_dependencies:
  - "@Development tools"
  - autoconf
  - automake
  - bash
  - dejagnu
  - gettext
  - grep
  - less
  - procps-ng
  - sed
  - tcl-devel

# RedHat-based distros: Centos 6
modules_package_dependencies:
  - "@Development tools"
  - autoconf
  - automake
  - bash
  - dejagnu
  - gettext
  - grep
  - less
  - procps
  - sed
  - tcl-devel
```

## Dependencies

None.

## Example Playbook

```yaml
- hosts: localhost
  roles:
    - role: jkglasbrenner.environment_modules
  vars:
    make: "make -j4"
    modules_use_own_dir: ".local/Modules"
    modules_example_modulefiles_to_remove:
      - dot
      - module-git
      - module-info
      - "{{ 'null' }}"
```


## License

MIT
