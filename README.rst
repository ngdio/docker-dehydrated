Docker-Dehydrated
=================
|Travis| |Stars| |Pulls| |Size|

This Docker image containerizes dehydrated,
a Let's Encrypt/ACME client implemented as a shell script.

Features
--------
* Based on Alpine Linux, which results in very small images
* Available on arm architectures

How to run
----------
**Command line:**

.. code:: bash

    docker volume create dehydrated_data
    docker volume create dehydrated_certs
    docker volume create dehydrated_www
    docker run -d \
        -v /etc/dehydrated:/dehydrated/config:ro
        -v dehydrated_data:/dehydrated/data
        -v dehydrated_certs:/dehydrated/certs
        -v dehydrated_www:/dehydrated/www
        ngdio/dehydrated

**docker-compose:**

.. code:: yaml

    version: "2"
    containers:
      dehydrated:
        image: ngdio/dehydrated
        restart: always
        volumes:
        - /etc/dehydrated:/dehydrated/config:ro
        - dehydrated_data:/dehydrated/data
        - dehydrated_certs:/dehydrated/certs
        - dehydrated_www:/dehydrated/www

    volumes:
      dehydrated_data:
      dehydrated_certs:
      dehydrated_www:

Volumes
-------
The :code:`/dehydrated` folder consists of four subfolders:

* :code:`/dehydrated/config`: contains the configuration files (see below).
  Please make sure the mounted folder contains the configuration files.
  You can find the standard configuration `in the repository`_, please
  adapt them to your needs.
* :code:`/dehydrated/www`: dehydrated will put the ACME challenges in this
  folder. Since dehydrated does not include a web server, you will have to
  mount this folder to a volume and share the challenges with a web server
  such as nginx or Apache. The folder must be made available to the public
  at :code:`/.well-known/acme-challenge/` via HTTP.
* :code:`/dehydrated/certs`: contains the certs obtained from Let's Encrypt.
  Each certificate is located in its own subfolder named after the
* :code:`/dehydrated/data`: data including leases and lock files which
  the user usually doesn't need to access. It is recommended to use a
  Docker volume for this folder.

.. _in the repository: https://github.com/ngdio/docker-dehydrated/tree/master/files/dehydrated/config/

Configuration Files
-------------------
The configuration folder (:code:`/dehydrated/config`) usually contains
three different files:

* :code:`/dehydrated/config/config`: the main configuration file. Adapt this
  to your needs.
  `(link to standard config)`_
* :code:`/dehydrated/config/domains.txt`: specifies for which domains
  certificates will be requested. dehydrated creates one certificate for each
  line, which can contain multiple domains seperated by spaces.
  `(link to standard domains.txt)`_
* :code:`/dehydrated/config/hook.sh`: shell script containing multiple
  functions which will be executed at specific times during the creation of
  certificates. For further information, take a look at the file.
  `(link to standard hook.sh)`_

.. _(link to standard config): https://github.com/ngdio/docker-dehydrated/blob/master/files/dehydrated/config/config
.. _(link to standard domains.txt): https://github.com/ngdio/docker-dehydrated/blob/master/files/dehydrated/config/domains.txt
.. _(link to standard hook.sh): https://github.com/ngdio/docker-dehydrated/blob/master/files/dehydrated/config/hook.sh

Contributions
-------------
* The automated build was adapted from `marthoc <https://github.com/marthoc>`_'s `deconz docker image <https://github.com/marthoc/docker-deconz/blob/master/.travis.yml>`_.
* The Dockerfile is based on NGINX Inc.'s `official Docker image for nginx <https://github.com/nginxinc/docker-nginx/>`_.

Quick reference
---------------
* **Where to file issues:**
    `https://github.com/ngdio/docker-dehydrated/issues <https://github.com/ngdio/docker-dehydrated/issues>`_

* **Maintained by:**
    `Niklas (ngdio) <https://github.com/ngdio>`_

* **Supported architectures:**
    `amd64 <https://github.com/ngdio/docker-dehydrated/blob/master/amd64/Dockerfile>`_, `i386 <https://github.com/ngdio/docker-dehydrated/blob/master/i386/Dockerfile>`_, `armhf <https://github.com/ngdio/docker-dehydrated/blob/master/armhf/Dockerfile>`_, `aarch64 <https://github.com/ngdio/docker-dehydrated/blob/master/aarch64/Dockerfile>`_

* **License:**
    The content of this repository is licensed under the `GPL 2-clause
    license`_. dehydrated is licensed under the `MIT license`_.

.. _GPL 2-clause license: https://github.com/ngdio/docker-dehydrated/blob/master/LICENSE
.. _MIT license: https://github.com/lukas2511/dehydrated/blob/master/LICENSE


.. |Travis| image:: https://img.shields.io/travis/ngdio/docker-dehydrated.svg?style=flat-square
   :target: https://travis-ci.org/ngdio/docker-dehydrated
   :alt: Travis
.. |Stars| image:: https://img.shields.io/docker/stars/ngdio/dehydrated.svg?style=flat-square
   :target: https://hub.docker.com/r/ngdio/dehydrated/
   :alt: Docker Stars
.. |Pulls| image:: https://img.shields.io/docker/pulls/ngdio/dehydrated.svg?style=flat-square
   :target: https://hub.docker.com/r/ngdio/dehydrated/
   :alt: Docker Pulls
.. |Size| image:: https://img.shields.io/microbadger/image-size/ngdio/dehydrated.svg?style=flat-square
   :target: https://hub.docker.com/r/ngdio/dehydrated/
   :alt: Image Size
