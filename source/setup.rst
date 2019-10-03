=====
Setup
=====

.. contents ::

Set up keyless ssh
==================

After add the public key to the remote machine, please make sure::

    > chmod g-w /home/your_user
    > chmod 700 /home/your_user/.ssh
    > chmod 600 /home/your_user/.ssh/authorized_keys

MET Docker and Singularity
==========================

* The MET Dockerfile and the configuration files are located at `MET Docker Github <https://github.com/Weatherhub/MET.git>`_

* Please checkout the `MET Docker <https://hub.docker.com/r/weatherhub/met/>`_ image with::

    > docker pull weatherhub/met

* Checkout the `MET Singularity <https://www.singularity-hub.org/collections/1372>`_ image with::

    > singularity pull shub://Weatherhub/MET

