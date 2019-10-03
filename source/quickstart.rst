==========
Quickstart
==========

.. contents ::

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

* Check out the LANGRUN MET Singularity Package::

    > singularity pull --name "met.img" shub://Weatherhub/MET
    > ls -la Weatherhub-MET-master.simg
    -rw-r--r--    1 xinzhang  staff  852807711 Oct 18 09:29 met.simg

* Convert PrepBufr observational data to netCDF format::

    > MET/pb2nc.py

.. note::
    please modify following paths to reflect your environment::

        gdas_dir='/nfs/data/raw/gdas'  # The path to the PrepBufr files
        nc_dir='/home/nwp/met/gdas_nc' # The path to the generated netCDF files
        os.system('/opt/singularity/2.4.2/bin/singularity ......  # The correct path to singularity executable

* Run the MET pointstat::

    > MET/pointstat.py

.. note::
    please modify following paths to reflect your environment::

        # where are the forecasts
        rap_dir='/home/nwp/rap/com/rap/prod'
        # where are the netcdf format observations
        gdas_nc='/home/nwp/met/gdas_nc'
        # where are the point stats
        ptstat_dir='/home/nwp/met/ptstat_nc'
        # The path to singularity exec.
        os.system('/opt/singularity/2.4.2/bin/singularity ....

* The above jobs can be scheduled in crontab::

    > crontab -l
    # convert gdas prepbufr data to netcdf format
    45 * * * * /home/nwp/met/pb2nc.py > log 2>&1
    # run point stat
    00 * * * * /home/nwp/met/pointstat.py > log2 2>&1
    