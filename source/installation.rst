============
软件的安装
============

.. contents ::

Installing Docker
=================

To build the *Docker* container, Docker need to be installed:

* please refer to `Docker Homepage <https://www.sylabs.io/>`_

Installing Docker Compose
==========================

To build the *Docker Compose*:

* please refer to `Docker compose Homepage <https://docs.docker.com/compose/install/>`_


Installing Charliecloud
=========================

The whole system requires to be run with *Charliecloud*. To install Charliecloud:

* please refer to `Charliecloud Homepage <https://hpc.github.io/charliecloud/index.html>`_


Obtain MET8.1 Charliecloud container
====================================

* please ask for MET8.1 Charliecloud container from provioder (Longrun weather)


Enter into MET8.1 Charliecloud container
=========================================

* Check out the MET configuration package::

    > git clone https://github.com/Weatherhub/MET.git
    > ls -la /Users/xinzhang/work/MET
    total 184
    drwxr-xr-x  20 xinzhang  staff   640 Oct 18 13:56 .
    drwxr-xr-x  20 xinzhang  staff   640 Oct 18 14:07 ..
    drwxr-xr-x  14 xinzhang  staff   448 Oct 17 08:20 .git
    -rw-r--r--   1 xinzhang  staff  7479 Oct 13 22:39 Dockerfile
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig0
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig1
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig2
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig3
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig4
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig5
    -rw-r--r--   1 xinzhang  staff  3873 Oct 13 22:39 PB2NCConfig6
    -rw-r--r--   1 xinzhang  staff  4777 Oct 13 22:39 PointStatConfig
    -rw-r--r--   1 xinzhang  staff   339 Oct 13 22:39 README.md
    -rw-r--r--   1 xinzhang  staff   314 Oct 13 22:39 Singularity
    -rw-r--r--   1 xinzhang  staff   151 Oct 13 22:39 docker-clean
    -rwxr-xr-x   1 xinzhang  staff  1274 Oct 13 22:39 pb2nc.py
    -rw-r--r--   1 xinzhang  staff  8984 Oct 13 22:39 plot_cnt.R
    -rw-r--r--   1 xinzhang  staff  8740 Oct 13 22:39 plot_mpr.R
    -rwxr-xr-x   1 xinzhang  staff  1068 Oct 13 22:39 pointstat.py


* enter into MET directory::

    > cd /Users/xinzhang/work/MET

* Build the met docker image::

    > docker image build -t Weatherhub/met .

* If successfully built, push the image to docker hub::

    > docker image push Weatherhub/met


Building LANGRUN MET Singularity image
======================================

* Build the met singularity image::

    > sudo singularity build met.simg Singualrity
