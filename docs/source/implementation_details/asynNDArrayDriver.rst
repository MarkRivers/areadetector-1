asynNDArrayDriver
=================

asynNDArrayDriver inherits from asynPortDriver. It implements the asynGenericPointer functions for NDArray objects. This is the class from which both plugins and area detector drivers are indirectly derived. The `asynNDArrayDriver class documentation <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/classasyn_n_d_array_driver.html>`_ describes this class in detail.

The file `asynNDArrayDriver.h <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/asyn_n_d_array_driver_8h.html>`_ defines a number of parameters that all NDArray drivers and plugins should implement if possible. These parameters are defined by strings (drvInfo strings in asyn) with an associated asyn interface, and access (read-only or read-write). There is also an integer index to the parameter which is assigned by asynPortDriver when the parameter is created in the parameter library. The EPICS database NDArrayBase.template provides access to these standard driver parameters. The following table lists the standard driver parameters. The columns are defined as follows:

- **Parameter index variable:** The variable name for this parameter index in the driver. There are several EPICS records in ADBase.template that do not have corresponding parameter indices, and these are indicated as Not Applicable (N/A).
- **asyn interface:** The asyn interface used to pass this parameter to the driver.
- **Access:** Read-write (r/w) or read-only (r/o).
- **drvInfo string:** The string used to look up the parameter in the driver through the drvUser interface. This string is used in the EPICS database file for generic asyn device support to associate a record with a particular parameter. It is also used to associate a `paramAttribute <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/classparam_attribute.html>`_ with a driver parameter in the XML file that is read by asynNDArrayDriver::readNDAttributesFile   
- **EPICS record name:** The name of the record in ADBase.template. Each record name begins with the two macro parameters $(P) and $(R). In the case of read/write parameters there are normally two records, one for writing the value, and a second, ending in _RBV, that contains the actual value (Read Back Value) of the parameter.
- **EPICS record type:** The record type of the record. Waveform records are used to hold long strings, with length (NELM) = 256 bytes and EPICS data type (FTVL) = UCHAR. This removes the 40 character restriction string lengths that arise if an EPICS "string" PV is used. MEDM allows one to edit and display such records correctly. EPICS clients will typically need to convert such long strings from a string to an integer or byte array before sending the path name to EPICS. In IDL this is done as follows:

::

          ; Convert a string to a null-terminated byte array and write with caput
          IDL> t = caput('13PS1:TIFF1:FilePath', [byte('/home/epics/scratch'),0B])
          ; Read a null terminated byte array 
          IDL> t = caget(`13PS1:TIFF1:FilePath`, v)
          ; Convert to a string 
          IDL> s = string(v)
          

In SPEC this is done as follows:

::

          array _temp[256]
          # Setting the array to "" will zero-fill it
          _temp = ""
          # Copy the string to the array.  Note, this does not null terminate, so if array already contains
          # a longer string it needs to first be zeroed by setting it to "".
          _temp = "/home/epics/scratch"
          epics_put("13PS1:TIFF1:FilePath", _temp)
          
Note that for parameters whose values are defined by enum values (e.g NDDataType, NDColorMode, etc.), drivers can use a different set of enum values for these parameters. They can override the enum menu in ADBase.template with driver-specific choices by loading a driver-specific template file that redefines that record field after loading ADBase.template. 


|  Parameter index variable        | asyn interface   |    Access       |        Description        |        drvInfo string        |        EPICS record name        |        EPICS record type           |        
|  cor_run_counter                 | asynInt32        |        R        |        Increments when image grab thread runs.         |        cor_run_counter        |        $(P)$(R)cor_run_counter        |        longin           |        
|  cor_frame_to_null               | asynInt32        |        WR       |        counts thrown away frames.         |        cor_frame_to_null        |        $(P)$(R)cor_frame_to_null_RBV $(P)$(R)cor_frame_to_null        |        longin longout           |        
|  cor_is_grab                     | asynInt32        |        WR       |        1 for grab mode, 0 does nothing.        |        cor_is_grab        |        $(P)$(R)cor_is_grab_RBV $(P)$(R)cor_is_grab        |        longin longout           |        
|  cor_num_coreco_buffers          | asynInt32        |        WR       |        Number of buffers on grabber.         |        cor_num_coreco_buffers        |        $(P)$(R)cor_num_coreco_buffers_RBV $(P)$(R)cor_num_coreco_buffers        |        longin longout           |        
|  cor_check_timestamps            | asynInt32        |        WR       |        check for lost frames by examening time stamps.         |        cor_check_timestamps        |        $(P)$(R)cor_check_timestamps_RBV $(P)$(R)cor_check_timestamps        |        longin longout           |        
|  cor_check_timestamps            | asynInt32        |        R        |        Descripttion         |        cor_check_timestamps        |        $(P)$(R)cor_num_buffers_RBV        |        longin           |        
|  cor_num_free_buffers            | asynInt32        |        R        |        have to fix this...         |        cor_num_free_buffers        |        $(P)$(R)cor_num_free_buffers_RBV        |        longin           |        
|  cor_is_abort                    | asynInt32        |        WR       |        1 to abort grab mode. 0 to do nothing.         |        cor_is_abort        |        $(P)$(R)cor_is_abort_RBV $(P)$(R)cor_is_abort        |        longin longout           |        
|  cor_grabber_type                | asynInt32        |        R        |        Tells which vender made grabber.         |        cor_grabber_type        |        $(P)$(R)cor_grabber_type_RBV        |        longin           |        
|  cor_is_freeze                   | asynInt32        |        WR       |        1 to freeze image grabbing.         |        cor_is_freeze        |        $(P)$(R)cor_is_freeze_RBV $(P)$(R)cor_is_freeze        |        longin longout           |        
|  cor_is_snap                     | asynInt32        |        WR       |        1 to snap one frame.         |        cor_is_snap        |        $(P)$(R)cor_is_snap_RBV $(P)$(R)cor_is_snap        |        longin longout           |        
|  cor_is_loadccf                  | asynInt32        |        W        |        write 1 to load grabber config. file.         |        cor_is_loadccf        |        $(P)$(R)cor_is_loadccf        |        longout           |        
|  cor_use_fpga                    | asynInt32        |        R        |        1 if we are using fpga on grabber. Only dalsa Andaconda has fpga.         |        cor_use_fpga        |        $(P)$(R)cor_use_fpga        |        mbbo           |        
|  cor_is_load_fpga_regs           | asynInt32        |        W        |        Load foga regs from file.         |        cor_is_load_fpga_regs        |        $(P)$(R)cor_is_load_fpga_regs        |        longout           |        
|  cor_is_read_fpga_regs           | asynInt32        |        W        |        Dump fpga registers to screen.         |        cor_is_read_fpga_regs        |        $(P)$(R)cor_is_read_fpga_regs        |        longout           |        
|  cor_ccf_filename                | asynOctetWrite   |        WR       |        Nema of config. file.         |        cor_ccf_filename        |        $(P)$(R)cor_ccf_filename $(P)$(R)cor_ccf_filename_RBV        |        stringout stringin           |        
|  cor_fpga_command                | asynInt32        |        R        |        Command sent to FPGA.         |        cor_fpga_command        |        $(P)$(R)cor_fpga_command_RBV        |        mbbi           |        
|  cor_fpga_num_imgs2ave           | asynInt32        |        WR       |        Number of frames for FPGA to average.         |        cor_fpga_num_imgs2ave        |        $(P)$(R)cor_fpga_num_imgs2ave $(P)$(R)cor_fpga_num_imgs2ave_RBV        |        longout longin           |        
|  cor_compression_rate            | asynInt32        |        WR       |        Compression rate done on FPGA.         |        cor_compression_rate        |        $(P)$(R)cor_compression_rate $(P)$(R)cor_compression_rate_RBV        |        longout longin           |        
|  cor_fpga__reg_filename          | asynOctetWrite   |        WR       |        Filename of FPGA register settings.         |        cor_fpga__reg_filename        |        $(P)$(R)cor_fpga_reg_filename $(P)$(R)cor_fpga_reg_filename_RBV        |        stringout stringin           |        
|  cor_is_sleep                    | asynInt32        |        W        |        1 to sleep every time image grab thread loops.         |        cor_is_sleep        |        $(P)$(R)cor_is_sleep        |        longout           |        
|  cor_sleep_ms                    | asynInt32        |        W        |        How long image thread sleeps between loops.         |        cor_sleep_ms        |        $(P)$(R)cor_sleep_ms        |        longout           |        
|  cor_fpga_filename               | asynOctetWrite   |        WR       |        name of FPGA firmware to load into FPGA,         |        cor_fpga_filename        |        $(P)$(R)cor_fpga_filename $(P)$(R)cor_fpga_filename_RBV        |        stringout stringin           |        
|  cor_input_filename              | asynOctetWrite   |        WR       |        to simulate camera, name if iamge to load into grabber from file.         |        cor_input_filename        |        $(P)$(R)cor_input_filename $(P)$(R)cor_input_filename_RBV                  | stringout stringin           |        
|  cor_dark_filename               | asynOctetWrite   |        WR       |        name if file to which dark image is saved.         |        cor_dark_filename        |        $(P)$(R)cor_dark_filename $(P)$(R)cor_dark_filename_RBV        |        stringout stringin           |        
|  cor_fpga_save_dark              | asynInt32        |        W        |        1 to dave darks.         |        cor_fpga_save_dark        |        $(P)$(R)cor_fpga_save_dark        |        longout           |        
|  cor_input_from_file             | asynInt32        |        R        |        1 to read image from disk into grabber.         |        cor_input_from_file        |        $(P)$(R)cor_input_from_file        |        mbbo           |        
|  cor_load_img_file               | asynInt32        |        W        |        deprecated?         |        cor_load_img_file        |        $(P)$(R)cor_load_img_file        |        longout           |        
|  cor_make_viewer                 | asynInt32        |        W        |        display debug viewer, from grabber library.          |        cor_make_viewer        |        $(P)$(R)cor_make_viewer        |        longout           |        
|  cor_sub_flag                    | asynInt32        |        R        |        1 for FPGA dark subtraction.         |        cor_sub_flag        |        $(P)$(R)cor_sub_flag_RBV        |        longin           |        
|  cor_comp_flag                   | asynInt32        |        R        |        1 for FPGA compression.         |        cor_comp_flag        |        $(P)$(R)cor_comp_flag_RBV        |        longin           |        
|  cor_acc_flag                    | asynInt32        |        R        |        1 for FPGA accumulation.         |        cor_acc_flag        |        $(P)$(R)cor_acc_flag_RBV        |        longin           |        
|  cor_store_flag                  | asynInt32        |        R        |        FPGA low level flag         |        cor_store_flag        |        $(P)$(R)cor_store_flag_RBV        |        longin           |        
|  cor_read_flag                   | asynInt32        |        R        |        FPGA low level flag          |        cor_read_flag        |        $(P)$(R)cor_read_flag_RBV        |        longin           |        
|  cor_saw_flag                    | asynInt32        |        R        |        FPGA low level flag          |        cor_saw_flag        |        $(P)$(R)cor_saw_flag_RBV        |        longin           |        
|  cor_dec_flag                    | asynInt32        |        R        |        FPGA low level flag          |        cor_dec_flag        |        $(P)$(R)cor_dec_flag_RBV        |        longin           |        
|  cor_abs_flag                    | asynInt32        |        R        |        FPGA low level flag          |        cor_abs_flag        |        $(P)$(R)cor_abs_flag_RBV        |        longin           |        
|  cor_comp_threshold              | asynInt32        |        WR        |        Threshiold for image compression.         |        cor_comp_threshold        |        $(P)$(R)cor_comp_threshold_RBV $(P)$(R)cor_comp_threshold        |        longin longout           |        
|  cor_frame_dec_rate              | asynInt32        |        WR        |        Compression rate.         |        cor_frame_dec_rate        |        $(P)$(R)cor_frame_dec_rate_RBV $(P)$(R)cor_frame_dec_rate        |        longin longout           |        
|  cor_load_dark_2_fpga            | asynInt32        |        W        |        Load dark from file into FPGA.         |        cor_load_dark_2_fpga        |        $(P)$(R)cor_load_dark_2_fpga        |        longout           |        
|  cor_coreco_message              | asynOctetRead        |        R        |        Error messages from grabber vendor library.         |        cor_coreco_message        |        $(P)$(R)cor_coreco_message_RBV        |        stringin           |        
|  cor_cant_get_ndarray            | asynInt32        |        WR        |        1 if we can't get ND array. Error condition.         |        cor_cant_get_ndarray        |        $(P)$(R)cor_cant_get_ndarray $(P)$(R)cor_cant_get_ndarray_RBV        |        longout longin           |        
|  cor_num_repeat_timestamp        | asynInt32        |        WR        |        1 if image timestamp is repeated, meaning repeated frame.         |        cor_num_repeat_timestamp        |        $(P)$(R)cor_num_repeat_timestamp_RBV $(P)$(R)cor_num_repeat_timestamp        |        longin longout           |        
|  cor_num_missed_timestamp        | asynInt32        |        WR        |        1 for missed timestamp, for missed frame. If timestamp increments longer than sime time, then we missed a stamp.         |        cor_num_missed_timestamp        |        $(P)$(R)cor_num_missed_timestamp_RBV $(P)$(R)cor_num_missed_timestamp        |        longin longout           |        
|  cor_missed_ts_wait              | asynInt32        |        WR        |        time above which we assume a missed frame.         |        cor_missed_ts_wait        |        $(P)$(R)cor_missed_ts_wait_RBV $(P)$(R)cor_missed_ts_wait        |        longin longout           |        
|  cor_nd_datasize                 | asynInt32        |        R        |        Data size in nd array.         |        cor_nd_datasize        |        $(P)$(R)cor_nd_datasize_RBV        |        longin           |        
|  cor_est_buffers_left            | asynInt32        |        R        |        Estimated ND Arraus left.         |        cor_est_buffers_left        |        $(P)$(R)cor_est_buffers_left_RBV        |        longin           |        
|  cor_max_ndbuffers               | asynInt32        |        R        |        Max ND Arrays we can store.         |        cor_max_ndbuffers        |        $(P)$(R)cor_max_ndbuffers_RBV        |        longin           |        
|  cor_max_ndmemory                | asynInt32        |        R        |        Max ND Arrauy memory.         |        cor_max_ndmemory        |        $(P)$(R)cor_max_ndmemory_RBV        |        longin           |        
|  cor_free_ndbuffers              | asynInt32        |        R        |        Free NDArraus.         |        cor_free_ndbuffers        |        $(P)$(R)cor_free_ndbuffers_RBV        |        longin           |        
|  cor_num_ndbuffers               | asynInt32        |        R        |        Number of created ND Arrauys.         |        cor_num_ndbuffers        |        $(P)$(R)cor_num_ndbuffers_RBV        |        longin           |        
|  cor_alloc_ndmemory              | asynInt32        |        R        |        Amount of created ND Array memory.         |        cor_alloc_ndmemory        |        $(P)$(R)cor_alloc_ndmemory_RBV        |        longin           |        
|  cor_total_missed_frames         | asynInt32        |        WR        |        counts total missed frames forever.         |        cor_total_missed_frames        |        $(P)$(R)cor_total_missed_frames $(P)$(R)cor_total_missed_frames_RBV        |        longout longin           |        
|  cor_recent_missed_frames        | asynInt32        |        WR        |        counts missed frames since last check.         |        cor_recent_missed_frames        |        $(P)$(R)cor_recent_missed_frames $(P)$(R)cor_recent_missed_frames_RBV        |        longout longin           |        
|  cor_num_buff_frames             | asynInt32        |        R        |        Number of buffers on grabber.         |        cor_num_buff_frames        |        $(P)$(R)cor_num_buff_frames_RBV        |        longin           |        
|  cor_num_bad_fpga_headers        | asynInt32        |        WR        |        1 for bad FPGA data.         |        cor_num_bad_fpga_headers        |        $(P)$(R)cor_num_bad_fpga_headers $(P)$(R)cor_num_bad_fpga_headers_RBV        |        longout longin           |        
|  cor_sap_buffer_count            | asynInt32        |        WR        |        Number of buffers in grabber.         |        cor_sap_buffer_count        |        $(P)$(R)cor_sap_buffer_count $(P)$(R)cor_sap_buffer_count_RBV        |        longout longin           |        
|  cor_comp_num_images             | asynInt32        |        R        |        Number of FPGA compressed iamges in one frame.         |        cor_comp_num_images        |        $(P)$(R)cor_comp_num_images_RBV        |        longin           |        
|  cor_comp_pixels0                | asynInt32        |        R        |        Num pixels in comp image.         |        cor_comp_pixels0        |        $(P)$(R)cor_comp_pixels0_RBV        |        longin           |        
|  cor_comp_pixels_error           | asynInt32        |        R        |        Error on compression.         |        cor_comp_pixels_error        |        $(P)$(R)cor_comp_pixels_error_RBV        |        longin           |        
|  cor_max_comp_pixels             | asynInt32        |        R        |        Max num pixels we can put into compressed iamge.         |        cor_max_comp_pixels        |        $(P)$(R)cor_max_comp_pixels_RBV        |        longin           |        
|  cor_compression_error           | asynInt32        |        R        |        Error on compression.         |        cor_compression_error        |        $(P)$(R)cor_compression_error_RBV        |        longin           |        
|  cor_num_thresh_std              | asynFloat64        |        WR        |        Number of std.deviations of noise for compression threshold.         |        cor_num_thresh_std        |        $(P)$(R)cor_num_thresh_std $(P)$(R)cor_num_thresh_std_RBV        |        ao ai           |        
|  cor_fpga_usr_command            | asynInt32        |        WR        |        Command sent to FPGA.         |        cor_fpga_usr_command        |        $(P)$(R)cor_fpga_usr_command $(P)$(R)cor_fpga_usr_command_RBV        |        mbbo mbbi           |        
|  cor_filename_in_fpga            | asynOctetRead        |        R        |        FPGA filename.         |        cor_filename_in_fpga        |        $(P)$(R)cor_filename_in_fpga_RBV        |        stringin           |        
|  cor_use_image_mode              | asynInt32        |        WR        |        Set FPGA algoration.         |        cor_use_image_mode        |        $(P)$(R)cor_use_image_mode_RBV $(P)$(R)cor_use_image_mode        |        longin longout           |        
|  cor_is_trigpin0                 | asynInt32        |        WR        |        Set grabber trigger pin 1, 0         |        cor_is_trigpin0        |        $(P)$(R)cor_is_trigpin0_RBV $(P)$(R)cor_is_trigpin0        |        longin longout           |        
|  cor_is_trigpin1                 | asynInt32        |        WR        |        Set grabber trigger pin 1, 0           |        cor_is_trigpin1        |        $(P)$(R)cor_is_trigpin1_RBV $(P)$(R)cor_is_trigpin1        |        longin longout           |        
|  cor_is_trigpin2                 | asynInt32        |        WR        |        Set grabber trigger pin 1, 0           |        cor_is_trigpin2        |        $(P)$(R)cor_is_trigpin2_RBV $(P)$(R)cor_is_trigpin2        |        longin longout           |        
|  cor_is_trigpin3                 | asynInt32        |        WR        |        Set grabber trigger pin 1, 0           |        cor_is_trigpin3        |        $(P)$(R)cor_is_trigpin3_RBV $(P)$(R)cor_is_trigpin3        |        longin longout
