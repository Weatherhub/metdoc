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

* activate and test::

    $ ch-tar2dir met81.tar.gz .
    $ ch-run met81 -- bash
    $ cd
    $ point_stat 

    *** Model Evaluation Tools (METV8.1.1) ***
    
    Usage: point_stat
    	fcst_file
    	obs_file
    	config_file
    	[-point_obs file]
    	[-obs_valid_beg time]
    	[-obs_valid_end time]
    	[-outdir path]
    	[-log file]
    	[-v level]
    
    	where	"fcst_file" is a gridded forecast file containing the field(s) to be verified (required).
    		    "obs_file" is an observation file in NetCDF format containing the verifying point observations (required).
    		    "config_file" is a PointStatConfig file containing the desired configuration settings (required).
    		    "-point_obs file" specifies additional NetCDF point observation files to be used (optional).
    		    "-obs_valid_beg time" in YYYYMMDD[_HH[MMSS]] sets the beginning of the matching time window (optional).
    		    "-obs_valid_end time" in YYYYMMDD[_HH[MMSS]] sets the end of the matching time window (optional).
    		    "-outdir path" overrides the default output directory (.) (optional).
    		    "-log file" outputs log messages to the specified file (optional).
    		    "-v level" overrides the default level of logging (2) (optional).


Obtain Metviewer Docker container
========================================

* please ask for Metviewer Charliecloud container from provioder (Longrun weather)


Load Metviewer Docker container
============================================

* activate and test::

    $ docker image load -i metviewer.tar.gz 
    877b494a9f30: Loading layer [==================================================>]  209.6MB/209.6MB
    97f2f5802dcb: Loading layer [==================================================>]  217.9MB/217.9MB
    cd1fe069da7a: Loading layer [==================================================>]  969.8MB/969.8MB
    1f0c11b0a00a: Loading layer [==================================================>]   2.56kB/2.56kB
    296d3c24a5e6: Loading layer [==================================================>]  7.981MB/7.981MB
    f7945d0b3f97: Loading layer [==================================================>]  13.93MB/13.93MB
    e64d268568d1: Loading layer [==================================================>]  47.43MB/47.43MB
    Loaded image: metviewer:latest

    $ docker image ls -a
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    metviewer           latest              0e8958dc48c4        6 days ago          1.42GB

Clone Metviewer repository
===========================

.. code-block:: bash

    $ git clone https://github.com/NCAR/container-dtc-metviewer.git


Up the Metviewer service
=========================

* make several directories and prepare the environmental variables, such as:

.. code-block:: bash

    $ mkdir -p mysql/tables
    $ export MYSQL_DIR=~/Longrun/MET/mysql/tables
    $ mkdir -p metviewer_output
    $ export METVIEWER_DIR=~/Longrun/MET/metviewer_output
    $ mkdir -p metviewer_data
    $ export METVIEWER_DATA=~/Longrun/MET/metviewer_data

.. code-block:: bash

    $ cd container-dtc-metviewer
    $ docker-compose up -d
    Pulling db (mysql:5.7)...
    5.7: Pulling from library/mysql
    8f91359f1fff: Pull complete
    6bbb1c853362: Pull complete
    e6e554c0af6f: Pull complete
    f391c1a77330: Pull complete
    414a8a88eabc: Pull complete
    fee78658f4dd: Pull complete
    9568f6bff01b: Pull complete
    76041efb6f83: Pull complete
    ea54dbd83183: Pull complete
    566857d8f022: Pull complete
    01c09495c6e7: Pull complete
    Digest: sha256:f7985e36c668bb862a0e506f4ef9acdd1254cdf690469816f99633898895f7fa
    Status: Downloaded newer image for mysql:5.7
    Creating mysql_mv ... done
    Creating metviewer_1 ... done

* Open a web browser (such as Firefox), open the website:

    http://localhost:8080/metviewer/metviewer1.jsp

    you will see the metviewer screen:
    
    .. figure:: images/metviewer_screen.png
       :scale: 70%
       :align: center
   