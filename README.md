# ansible-role-jmeter
=========

[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-jmeter/master/LICENSE)
[![Build Status](https://travis-ci.org/lean-delivery/ansible-role-jmeter.svg?branch=master)](https://travis-ci.org/lean-delivery/ansible-role-jmeter)
[![Build Status](https://gitlab.com/lean-delivery/ansible-role-jmeter/badges/master/pipeline.svg)](https://gitlab.com/lean-delivery/ansible-role-jmeter/pipelines)
[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.jmeter-blue.svg)](https://galaxy.ansible.com/lean_delivery/jmeter)
![Ansible](https://img.shields.io/ansible/role/d/46312.svg)
![Ansible](https://img.shields.io/badge/dynamic/json.svg?label=min_ansible_version&url=https%3A%2F%2Fgalaxy.ansible.com%2Fapi%2Fv1%2Froles%2F46312%2F&query=$.min_ansible_version)

# Table Of Contents

* [About](#about)
* [Requirements](#requirements)
* [Dependencies](#dependencies)
* [Role Parameters](#role-parameters)
* [Installation](#installation)
* [Example Playbook](#example-playbook)
* [License](#license)
* [Author](#author)

## About

> Ansible role to install Apache Jmeter with plugins

## Requirements

**Supported OS**:

   - Ubuntu
     - bionic
     - xenial
   - Debian
     - stretch
   - Amazon Linux 2
   - EL
     - 7

**Minimal Ansible version**:

  - 2.8

[Back to table of contents](#table-of-contents)

## Dependencies

**Java 8**   
You can use any version you like - OpenJDK or Oracle SE Java. It can be installed manually or using ansible roles like this:

[![Galaxy](https://img.shields.io/badge/galaxy-lean__delivery.java-blue.svg)](https://galaxy.ansible.com/lean_delivery/java)

[Back to table of contents](#table-of-contents)

## Role Parameters

**Quicklist**: [jmeter_version](#jmeter_version),
[jmeter_binaries_url](#jmeter_binaries_url),
[jmeter_checksum](#jmeter_checksum),
[jmeter_checksum_url](#jmeter_checksum_url),
[jmeter_config_properties](#jmeter_config_properties),
[jmeter_package](#jmeter_package), [jmeter_root_path](#jmeter_root_path),
[jmeter_report_path](#jmeter_report_path), [jmeter_path](#jmeter_path),
[jmeter_plugins](#jmeter_plugins),
[jmeter_plugins_install](#jmeter_plugins_install),
[jmeter_plugins_manager_action](#jmeter_plugins_manager_action),
[jmeter_plugins_manager_version](#jmeter_plugins_manager_version),
[jmeter_cmdrunner_version](#jmeter_cmdrunner_version),
[jmeter_tmp_folder](#jmeter_tmp_folder)

### jmeter_version

* *help*: Version of Jmeter to install (e.g. `5.2.1`). If not defined explicitly, the latest available version will be installed.
* *default*: **undefined**

[Back to table of contents](#table-of-contents)

### jmeter_binaries_url 

* *help*: Root url to get jmeter binaries from. You can redefine it to alternative mirror if you like.
* *default*: ``https://archive.apache.org/dist/jmeter/binaries``

[Back to table of contents](#table-of-contents)

### jmeter_checksum 

* *help*: Checksum to validate downloaded binary. Default value is taken from Apache repository specified by `jmeter_checksum_url`.
* *default*: ``{{ lookup('url', jmeter_checksum_url).split()[0] }}``

[Back to table of contents](#table-of-contents)

### jmeter_checksum_url 

* *help*: Link to checksum url to validate downloaded binary.
* *default*: ``https://archive.apache.org/dist/jmeter/binaries/{{ jmeter_package }}.sha512``

[Back to table of contents](#table-of-contents)

### jmeter_config_properties 

* *help*: List of dictionaries with configuration properties. You can specify different configuration files by `name` key and corresponding parameters by `properties` key with list of dictionaries.
* *default*: 
  * ``{'name': 'upgrade'}``
  * ``{'name': 'system'}``
  * ``{'name': 'jmeter'}``
  * ``{'name': 'reportgenerator'}``
  * ``{'name': 'saveservice'}``
  * ``{'name': 'user'}``

  For example:
```yaml
    jmeter_config_properties:
      - name: system
        properties:
          - key: networkaddress.cache.negative.ttl
            value: 10
          - key: javax.net.debug
            value: ssl
```

[Back to table of contents](#table-of-contents)

### jmeter_package 

* *help*: Jmeter archive name to download and install.
* *default*: ``apache-jmeter-{{ jmeter_version }}.tgz``

[Back to table of contents](#table-of-contents)

### jmeter_root_path 

* *help*: Folder where Jmeter subfolder is placed.
* *default*: ``/opt``

[Back to table of contents](#table-of-contents)

### jmeter_report_path 

* *help*: Path to folder with generated reports.
* *default*: ``{{ jmeter_root_path }}/reports``

[Back to table of contents](#table-of-contents)

### jmeter_path 

* *help*: Jmeter home folder path.
* *default*: ``{{ jmeter_root_path }}/apache-jmeter-{{ jmeter_version }}``

[Back to table of contents](#table-of-contents)

### jmeter_plugins 

* *help*: List of Jmeter plugins to be installed.
* *default*: []

[Back to table of contents](#table-of-contents)

### jmeter_plugins_install 

* *help*: Controls option to install additional plugins. If `true` - plugins manager will be installed. Then additional plugins will be installed specified by `jmeter_plugins` list variable.
* *default*: ``false``

[Back to table of contents](#table-of-contents)

### jmeter_plugins_manager_action 

* *help*: Jmeter plugin manager action to perform. Available options "install", "install-all-except", "uninstall".
* *default*: ``install``

[Back to table of contents](#table-of-contents)

### jmeter_plugins_manager_version 

* *help*: Plugin manager library version.
* *default*: ``latest``

[Back to table of contents](#table-of-contents)

### jmeter_cmdrunner_version 

* *help*: Java library 'cmdrunner' version.
* *default*: ``latest``

[Back to table of contents](#table-of-contents)

### jmeter_tmp_folder 

* *help*: Folder to store downloaded files during installation.
* *default*: ``/tmp``

[Back to table of contents](#table-of-contents)

# Installation

`ansible-galaxy install lean_delivery.jmeter`

## Example Playbook

```yaml
- name: Install Java and Jmeter
  hosts: all
  roles:

    - role: lean_delivery.java
      java_distribution: openjdk
      java_major_version: 8
      transport: repositories
      java_tarball_install: false

    - role: lean_delivery.jmeter
      jmeter_plugins_install: true
      jmeter_plugins:
        - jpgc-casutg
        - jpgc-tst
        - jpgc-functions
        - jpgc-dummy
```
[Back to table of contents](#table-of-contents)

## License

Apache

[![License](https://img.shields.io/badge/license-Apache-green.svg?style=flat)](https://raw.githubusercontent.com/lean-delivery/ansible-role-jmeter/master/LICENSE)

[Back to table of contents](#table-of-contents)

## Author Information

Lean Delivery Team <team@lean-delivery.com>

[Back to table of contents](#table-of-contents)
