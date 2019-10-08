空间诊断分析 （MOD）
==========================

.. contents ::

configure
-----------

The behavior of MODE is controlled by the contents of the configuration file passed to it on the command line. The default MODE configuration file may be found in the :code:`/met/met-8.1.1/data/config/MODEConfig_default` file. The configurations used by the test scripts may be found in the $MET_BASE/config/MODEConfig* files. Prior to modifying the configuration file, users are advised to make a copy of the default:

.. code-block:: bash

    $ cp $MET_BASE/config/MODEConfig_default $MET_TUTORIAL_DATA/config/MODEConfig_tutorial

The configuration items for MODE are used to specify how the object-based verification approach is to be performed. Whereas in Point-Stat and Grid-Stat you may only compare the same type of forecast and observation fields, in MODE you may compare any two fields. When necessary, the items in the configuration file are specified separately for the forecast and observation fields. In most cases though, users will be comparing the same forecast and observation fields. The configurable items include specifications for the following:

    1. The verification domain.
    #. The forecast and observation fields and vertical levels or accumulation intervals to be compared.
    #. Options to censor a portion of or threshold the raw fields.
    #. The forecast and observation object definition parameters.
    #. Options to filter out objects that don't meet a size or intensity criteria.
    #. Flags to control the logic for matching/merging.
    #. Weights to be applied for the fuzzy engine matching/merging algorithm.
    #. Interest functions to be used for the fuzzy engine matching/merging algorithm.
    #. Total interest threshold for matching/merging.
    #. Various plotting options.

While the MODE configuration file contains many options, beginning users will typically only need to modify a few of them. You may find a complete description of the configurable items in the MET Users Guide or in the :code:`$MET_BASE/config/README` file. Please take some time to review them.

For this tutorial, we'll configure MODE to verify the same 12-hour accumulated precipitation output of PCP-Combine that we used for Grid-Stat. Whereas Grid-Stat and Point-Stat may be used to compare multiple fields in one run, MODE compares a single forecast field to a single observation field.

Open up the :code:`$MET_TUTORIAL_DATA/config/MODEConfig_tutorial` file for editing with the text editor of your choice and edit it as follows:

    * Set :code:`grid_res = 40`;
        To set the nominal grid spacing to 40km for this grid. The grid_res parameter is used further down in the config file in defining interest functions.
    * In the fcst dictionary, set

        .. code-block:: bash

            field = {
               name  = "APCP_12";
               level = "(*,*)";
            };

        To select the forecast field of 12-hour rainfall total accumulation from the input NetCDF file.

    * In the fcst dictionary, set :code:`conv_radius = 5`;
        To specify a convolution smoothing radius of 5 grid units. This parameters may be set explicitly, as we're doing now, or relative to another value, like the :code:`grid_res` parameter which was set above.
    * In the fcst dictionary, set :code:`conv_thresh = >=5.0`;
        To threshold the convolved field and define objects.
    * In the fcst dictionary, set :code:`merge_flag = NONE`;
        To disable the additional forecast and observation merging methods.
    * Set :code:`obs = fcst`;
        To use the settings from the fcst dictionary above.
    * Set :code:`match_flag = MERGE_BOTH`;
        To use the one-step matching/merging method.

run
----------

Next, run MODE on the command line using the following command:

.. code-block:: bash

   $ mode \
    $MET_TUTORIAL_DATA/output/pcp_combine/sample_fcst_24L_2005080800V_12A.nc \
    $MET_TUTORIAL_DATA/output/pcp_combine/sample_obs_2005080800V_12A.nc \
    $MET_TUTORIAL_DATA/config/MODEConfig_tutorial \
    -outdir $MET_TUTORIAL_DATA/output/mode \
    -v 2

MODE is now performing the verification task we requested in the configuration file. It should take a minute or two to run. MODE's runtime is greatly influenced by the number of gridpoints in the domain, the convolution radius chosen, and the number of objects resolved. The more dense the domain, larger the convolution radius, and greater the number of objects, the more computations required.

When MODE is finished, it will have created four files: two ASCII statistics files, a NetCDF object file, and a PostScript summary plot. Open up the PostScript summary plot using the PostScript viewer of your choice, :code:`gv`, or :code:`Ghostview`, for example:

.. code-block:: bash

    $ gv $MET_TUTORIAL_DATA/output/mode/mode_240000L_20050808_000000V_120000A.ps

This PostScript summary plot contains 5 pages. The first page summarizes the application of MODE to this dataset. The second and third pages contain enlargements of the forecast and observation raw and object fields. The fourth page shows the forecast and observation object fields overlaid on top of each other. And the fifth page contains pair-wise differences for the matched clusters of objects. The PostScript summary plot will contain additional pages when additional merging methods are selected. Looking at the first page, note the following:

    1. The valid data in the forecast field extends much further than in the observation field leading to objects in the forecast field with no match (royal blue = unmatched) in the observation field.
    #. The forecast field contains 5 objects while the observation field contains 6.
    #. Two pairs of objects (colored red and green) are matched across these fields. Forecast object 4 matches observed object 5 (red). Forecast object 3 matches observed object 2 (green).

output
----------------

As mentioned on the previous page, the output of MODE typically consists of four files: two ASCII statistics files, one NetCDF object file, and one PostScript summary plot. The output of any of these files may be disabled using the appropriate configuration file entry. In this example, the output is written to the :code:`$MET_TUTORIAL_DATA/output/mode` directory as we requested on the command line.

The MODE output file naming convention is designed to contain the lead times, valid times, and accumulation times. If you rerun MODE on the same fields but with a slightly different configuration, the new output will override the old output, unless you redirect it to a different directory using the :code:`-outdir` command line argument or specify an :code:`output_prefix` in the configuration file. The four MODE output files are described briefly below:

    1. The PostScript file ends in .ps and was described on the previous page.
    #. The NetCDF object file ends in :code:`_obj.nc` and contains the raw and cluster object indices and boundary polylines for the simple objects.
    #. The ASCII contingency table statistics file ends in :code:`_cts.txt`.
    #. The ASCII object statistics file ends in :code:`_obj.txt` and contains all of the object and object comparison data.

Since we've already seen the PostScript summary plot, we'll skip that one here. Use the ncview utility (if available on your machine) to view the NetCDF object output of MODE:

.. code-block:: bash

    $ ncview $MET_TUTORIAL_DATA/output/mode/mode_240000L_20050808_000000V_120000A_obj.nc&

Click through the variable names in the ncview window to see plots of the four object fields in the file. The :code:`fcst_obj_id` and :code:`obs_obj_id` contain the indices for the forecast and observation objects defined by MODE. The :code:`fcst_clus_id` and :code:`obs_clus_id` contain indices for the matched cluster objects. Now dump the header:

.. code-block:: bash

    $ ncdump -h $MET_TUTORIAL_DATA/output/mode/mode_240000L_20050808_000000V_120000A_obj.nc

View the NetCDF header to see how the file is structured.

The object colors plotted by ncview will generally not correspond to those in MODE's PostScript output.

Next, open up the :code:`$MET_TUTORIAL_DATA/output/mode/mode_240000L_20050808_000000V_120000A_cts.txt` contingency table statistics ASCII file using the text editor of your choice. This file is similar to the CTS output of Grid-Stat but much less complete. It contains three lines, a header row followed by contingency table statistics computed two ways:

    1. The first row contains *RAW* in the *FIELD* column. The scores listed in this row are computed from the *RAW* forecast and observation fields. The raw fields are thresholded using the fcst_conv_thresh and obs_conv_thresh values specified to create 0/1 mask fields. Those mask fields are compared point by point to compute a contingency table. The scores listed in this row are derived from that contingency table.
    #. The second row contains OBJECT in the *FIELD* column. The scores listed in this row are computed from the forecast and observation OBJECT fields. In MODE, after objects have been defined, the field may be thought of as a 0/1 mask field, 1 at grid points contained inside an object and 0 everywhere else. The object mask fields are compared in this way point by point, a contingency table is computed, and the corresponding statistics are listed in this row.

This file is not meant to replicate or replace the functionality of the Grid-Stat tool which includes many more features and options. It is simply meant to provide a convenient way of seeing how the output of MODE compares to the traditional contingency table statistics that are often computed.

Close this file, and open up the :code:`$MET_TUTORIAL_DATA/output/mode/mode_240000L_20050808_000000V_120000A_obj.txt` object statistics ASCII file using the text editor of your choice. This file contains all of the object statistics in which most users will be interested. It contains four different line types which may be distinguished by the contents of the *OBJECT_ID* column:

    1. The rows containing F### and O### in that column give information about the simple forecast and observation objects, respectively. ### refers to the simple object number (e.g. "F001" for the first simple forecast object or "O010" for the tenth simple observation object).
    #. The rows containing F###_O### in that column give information about pairs of simple objects (e.g. "F001_O010" compares the first forecast object to the tenth observation object).
    #. The rows containing CF### and CO### in that column give information about the cluster forecast and observation objects, respectively. ### refers to the cluster object number.
    #. The rows containing CF###_CO### in that column give information about pairs of cluster objects.

In the ASCII MODE statistics file, the value of 000 for NNN in the *OBJECT_ID* column indicates that that object was not matched.

Each line in this file contains the same number of columns. However, only certain columns are applicable to certain line types. For example, the CENTROID_X and CENTROID_Y columns contain valid data for simple object lines, but not for pairs of simple object lines. The opposite is true for the CENTROID_DIST column which gives the distance between the centroids of two objects. Columns which are not applicable to a given line type are filled with a value of NA.
