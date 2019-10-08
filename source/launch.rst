运行MET
==============

.. contents ::

获取 MET8.1 Charliecloud container
---------------------------------------

* please ask for MET8.1 Charliecloud container from provioder (Longrun weather)


启动 MET8.1 Charliecloud container
--------------------------------------

.. code-block:: bash

    $ ch-tar2dir met81-charliecloud.tar.gz .
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
