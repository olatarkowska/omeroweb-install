[![Build Status](https://travis-ci.org/ome/omeroweb-install.svg?branch=master)](https://travis-ci.org/ome/omeroweb-install)


OMERO.web installation scripts
==============================

Example of OMERO.web installation scripts for Linux: CentOS7, Ubuntu 16.04, Debian 9
and Mac OS X.
These scripts are provided to help new users with installing OMERO.web for the
first time on a clean system, and can be used as the basis for more advanced
configurations.

Read the OMERO System Administrator Documentation https://docs.openmicroscopy.org/latest/omero/sysadmins/index.html,
especially the sections on requirements and installation, before using these scripts.

The Linux and OS X scripts should automatically download all required files.

The scripts will only work locally due to the usage of ``{{playbook_dir}}``.
This ensures that the generated documentation will be created in the same location
regardless of the version of Ansible installed.

Prerequisites
-------------

    pip install ansible


Building
--------

    ansible-playbook ./ansible/omeroweb-install.yml -i ./ansible/hosts/centos7-ice3.6

to clean up existing scripts:

    ansible-playbook ./ansible/omeroweb-install.yml -i ./ansible/hosts/centos7-ice3.6 --extra-vars "clean=True"

Scripts are autogenerated using ansible, that means they can build directly on the remote host.
Create your own copy of inventory, e.g. `ansible/hosts/centos7-ice3.6`  and define a list of hosts

    cat << EOF > /path/to/hosts/centos7-ice3.6
    [centos7-ice3.6]
    omeroweb1.example.com ansible_port=22 ansible_host=omeroweb1.example.com ansible_user=username ansible_ssh_pass=secret
    EOF

    ansible-playbook ./ansible/omeroweb-install.yml -i /path/to/hosts/centos7-ice3.6


Extra vars arguments:

| ARG                | default | optional                | example                  |
|--------------------|---------|-------------------------|--------------------------|
| omero_version      | latest  |                         | OMERO-DEV-latest         |
| web_port           | 80      |                         |                          |
| web_prefix         |         |                         | /omero                   |
| web_server_name    |         |                         | omero.example.com        |
| web_session        | False   | True|False              |                          |
| clean              | False   | True|False              |                          |

example usage:

    ansible-playbook ./ansible/omeroweb-install.yml -i ./hosts/centos7-ice3.6 --extra-vars "web_prefix=/omero web_server_name=IP_GOES_HERE web_port=80"

Running
-------

Environment variables:

| VAR            | default | optional                | example                  |
|----------------|---------|-------------------------|--------------------------|
| OMEROVER       | latest  |                         | OMERO-DEV-latest |
| WEBPORT        | 80      |                         |                          |
| WEBPREFIX      |         |                         | /omero                   |
| WEBSERVER_NAME |         |                         | omero.example.com        |
| WEBSESSION     | False   | True|False              |                          |


and run

on Mac OS X:

    ./omeroweb-install-osx-ice3.6
    ./osx/run

on Ubuntu 16.04:

    ./omeroweb-install-ubuntu1604-ice3.6
    ./ubuntu/run

on CentOS 7:

    ./omeroweb-install-centos7-ice3.6

To run installation scripts on a remote host:

    $ ssh bob@hostname
    $ mv centos7-ice3.6 omeroweb-install-centos7-ice3.6 /tmp/
    $ sudo /tmp/omeroweb-install-centos7-ice3.6  ## absolute path is required


Testing in DOCKER
-----------------

These tests are only for CentOS 7, Ubuntu, and Debian deployments. Unfortunately there is no docker container for Mac OS X installation scripts

    OS=centos7 ICEVER=3.6 OMEROVER=OMERO-DEV-latest WEBPREFIX=/omero TRAVIS=False .travis/before_install.sh
    OS=centos7 ICEVER=3.6 OMEROVER=OMERO-DEV-latest WEBPREFIX=/omero DOCKER=true TRAVIS_OS_NAME=linux .travis/install.sh
    OS=centos7 ICEVER=3.6 OMEROVER=OMERO-DEV-latest WEBPREFIX=/omero DOCKER=true TRAVIS_OS_NAME=linux .travis/script.sh 
 
To test remote build set `ANSIBLE=true`
Note: make sure you always set `DOCKER=true` when running local test. Otherwise it will attempt to install OMERO.web on you local machine

Note: running tests on Mac OS X requires Docker for Mac (Docker Toolbox is not supported)

Copyright
---------

2016-2019, The Open Microscopy Environment
