# AHWRF Running on HPC

```console
$ cd $HOME
$ mkdir runhwrf
$ cd runhwrf/
$ export user=hwrf3
$ ln -sf /home/${user}/WRFV4.7.0/run/* .
$ rm namelist.input*
$ cp /home/${user}/WRFV4.7.0/run/namelist.input .
$ ln -sf /home/${user}/WPS/*exe .
$ ln -sf /home/${user}/WPS/link_grib.csh .
$ ln -sf /home/${user}/WPS/geogrid .
$ ln -sf /home/${user}/WPS/ungrib .
$ ln -sf /home/${user}/WPS/metgrid .
$ cp /home/${user}/WPS/namelist.wps .
$ ln -sf /home/${user}/WPS/ungrib/Variable_Tables/Vtable.GFS Vtable
$ cd geogrid
$ ln -sf GEOGRID.TBL.ARW GEOGRID.TBL
$ cd ../metgrid
$ ln -sf METGRID.TBL.ARW METGRID.TBL
$ cd ..
$ ./link_grib.csh /opt/media-ext1/hwrf/GDAS-TestRun/gdas1.fnl0p25.202104*f00.grib2
$ vi namelist.wps
```

```console
&share
 wrf_core = 'ARW',
 max_dom = 1,
 start_date = '2021-04-04_00:00:00','2021-04-04_00:00:00','2021-04-04_00:00:00',
 end_date   = '2021-04-13_00:00:00','2021-04-13_00:00:00','2021-04-13_00:00:00',
 interval_seconds = 21600
 io_form_geogrid = 2,
/

&geogrid
 parent_id         = 1,1,2,
 parent_grid_ratio = 1,3,3,
 i_parent_start    = 1,86,82,
 j_parent_start    = 1,86,82,
 e_we          = 250,238,226,
 e_sn          = 250,238,226,
 geog_data_res = '30s','30s','30s',
 dx = 18000,
 dy = 18000,
 map_proj =  'mercator',
 ref_lat   = -11,
 ref_lon   = 122,
 truelat1  = -11,
 truelat2  = 0,
 stand_lon = 122,
 geog_data_path = '/opt/media-ext1/mpas/GEOG/'
 ref_x = 125.0,
 ref_y = 125.0,
/

&ungrib
 out_format = 'WPS',
 prefix = 'FILE',
/

&metgrid
 fg_name = 'FILE'
/

```

```console
$ ./geogrid.exe
$ ./ungrib.exe
$ ./metgrid.exe
$ nano namelist.input

```

```console
 &time_control
 run_days                            = 108,
 run_hours                           = 0,   
 run_minutes                         = 0,
 run_seconds                         = 0,
 start_year                          = 2021,     2021, 2021, 
 start_month                         = 04,       04, 04,
 start_day                           = 04,       04, 04,
 start_hour                          = 00,       00, 00,
 start_minute                        = 00,       00, 00,
 start_second                        = 00,       00, 00,
 end_year                            = 2021,     2021, 2021,
 end_month                           = 04,       04, 04,
 end_day                             = 13,       13, 13,
 end_hour                            = 00,       00,	00,
 end_minute                          = 00,       00,	00,
 end_second                          = 00,       00,	00,
 input_from_file					 = .true.,	.false.,	.false.,
 interval_seconds                    = 21600,
 history_interval                    = 60,       60,	60,
 frames_per_outfile                  = 1000, 1000, 1000,
 history_outname 					 = "/opt/media-ext1/hwrf/hwrf3/seroja2/wrfout_d<domain>_<date>"
 restart                             = .false.,
 restart_interval                    = 7200,
 io_form_history                     = 2
 io_form_restart                     = 2
 io_form_input                       = 2
 io_form_boundary                    = 2
 /

 &domains
 time_step                           = 45,
 time_step_fract_num                 = 0,
 time_step_fract_den                 = 1,
 max_dom                             = 3,
 e_we                                = 250,238,226,
 e_sn                                = 250,238,226,
 e_vert                              = 48,    48,    48,
 dzbot                               = 30.
 dzstretch_s                         = 1.11
 dzstretch_u                         = 1.10
 p_top_requested                     = 5000,
 num_metgrid_levels                  = 34,
 num_metgrid_soil_levels             = 4,
 dx                                  = 18000,6000,2000,
 dy                                  = 18000,6000,2000,
 grid_id                             = 1,2,3,
 parent_id                           = 1,1,2,
 i_parent_start                      = 1,86,82,
 j_parent_start                      = 1,86,82,
 parent_grid_ratio                   = 1,3,3,
 parent_time_step_ratio              = 1,3,3,
 feedback                            = 1,
 smooth_option                       = 0,
 vortex_interval		     = 15
 max_vortex_speed		     = 40
 corral_dist			     = 8
 track_level			     = 50000
 time_to_move			     = 0
 /

 &physics
 mp_physics                          = 3,     3,     3,
 ra_lw_physics                       = 1,     1,     1,
 ra_sw_physics                       = 1,     1,     1,
 radt                                = 30,    30,    30,
 sf_sfclay_physics                   = 1,     1,     1,
 sf_surface_physics                  = 2,     2,     2,
 bl_pbl_physics                      = 1,     1,     1,
 bldt                                = 0,     0,     0,
 cu_physics                          = 1,     1,     0,
 cudt                                = 5,     5,     5,
 isfflx                              = 1,
 ifsnow                              = 0,
 icloud                              = 1,
 surface_input_source                = 1,
 num_soil_layers                     = 4,
 sf_urban_physics                    = 0,     0,     0,
 maxiens                             = 1,
 maxens                              = 3,
 maxens2                             = 3,
 maxens3                             = 16,
 ensdim                              = 144,
 /

 &fdda
 /

 &dynamics
 hybrid_opt                          = 2, 
 w_damping                           = 0,
 diff_opt                            = 2,      2,	2,
 km_opt                              = 4,      4,	4,
 diff_6th_opt                        = 0,      0,	0,
 diff_6th_factor                     = 0.12,   0.12,	0.12,
 base_temp                           = 290.
 damp_opt                            = 3,
 zdamp                               = 5000.,  5000.,	5000.,
 dampcoef                            = 0.2,    0.2,	0.2,
 khdif                               = 0,      0,
 kvdif                               = 0,      0,
 non_hydrostatic                     = .true., .true.,	.true.,
 moist_adv_opt                       = 1,      1,
 scalar_adv_opt                      = 1,      1,
 gwd_opt                             = 1,      0,
 /

 &bdy_control
 spec_bdy_width                      = 5,
 specified                           = .true.
 /

 &grib2
 /

 &namelist_quilt
 nio_tasks_per_group = 0,
 nio_groups = 1,
 /

```

## Running AHWRF process
```console
$ ./real.exe
$ mpirun -n 2 ./wrf.exe &
$ tail -f rsl.out.0000

```

## Check the results
Check the header of entire file
```console
$ ncdump -h wrfout_d01_2021-04-04_00:00:00
```
Check the visualization of entire file
```console
$ ncview wrfout_d01_2021-04-04_00:00:00
```
Example of another visualization using python script
```console
import xarray as xr
import cartopy.crs as ccrs
import matplotlib.pyplot as plt

ds=xr.open_dataset('/opt/media-ext1/hwrf/hwrf3/seroja2/wrfout_d03_2021-04-04_00:00:00')
press=ds['PSFC']
psfc=press.values
lat=press.XLAT.values
lon=press.XLONG.values
xti=press.XTIME.values
ax = plt.axes(projection=ccrs.PlateCarree())
ax.coastlines()
ax.set_extent([100, 130, -20, -5])
ax.gridlines(draw_labels=True,color='black',alpha=0.5,linestyle='--')
cs=ax.contourf(lon[100],lat[100],psfc[100]/100,cmap= 'viridis', transform=ccrs.PlateCarree())
plt.title(str(xti[100])[:13]+' UTC');
plt.colorbar(cs,orientation='horizontal')
plt.savefig('maptrack.png')
plt.show()
```
Example of ploting track of hwrf output using python script

```
$ grep ATCF rsl.out.0000 > htrack.txt
```
```console
import cartopy.crs as ccrs
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

ds=pd.read_csv('/opt/media-ext1/hwrf/hwrf3/seroja2/htrack.txt', header=None)
lo=[]
la=[]
for i in range(len(ds)):
    da=ds[0][i].split()
    la.append(float(da[2]))
    lo.append(float(da[3]))
lo=np.array(lo)
la=np.array(la)

ax = plt.axes(projection=ccrs.PlateCarree())
ax.coastlines()
ax.set_extent([100, 130, -20, -5])
ax.gridlines(draw_labels=True,color='black',alpha=0.5,linestyle='--')
plt.plot(lo,la,'o-')
plt.savefig('track.png')

```
## Finish
