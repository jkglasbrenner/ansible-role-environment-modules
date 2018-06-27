# Ansible Role: Environment Modules

Builds [environment modules](http://modules.sourceforge.net/) from source on Debian and Ubuntu installations.
Environment modules are used for dynamic modification of a user's environment via modulefiles and is typically installed on HPC clusters.

## Requirements

None.

## Role Variables

Variables and default values:

```yaml
# To use more CPUs during 
make_cmd: make
workspace: "{{ ansible_env.HOME }}/src"
modules_src_sha256: d1f54f639d1946aa1d7ae8ae03752f8ac464a879c14bc35e63b6a87b8a0b7522
modules_install_from_source: true
modules_install_path: "/usr/local/Modules"
modules_modulefiles_path: "/usr/local/Modules/modulefiles"
modules_version: 4.1.2
modules_install_from_source_force_update: true
modules_reinstall_from_source: false
modules_use_own_directory: privatemodules
modules_example_modulefiles_to_remove: []
```

Distribution-specific variables:

```yaml
# Debian/Ubuntu
modules_install_from_source_dependencies:
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
```

## Dependencies

None.

## Example Playbook

```yaml
- hosts: localhost
  roles:
    - role: jkglasbrenner.environment-modules
  vars:
    workspace: "{{ ansible_env.HOME }}/.local/src"
    make: "make -j4"
    modules_use_own_directory: ".local/Modules"
    modules_example_modulefiles_to_remove:
      - dot
      - module-git
      - module-info
      - "{{ 'null' }}"
```


## License

MIT
