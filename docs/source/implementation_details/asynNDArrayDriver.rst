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


+-------------------------------+----------------------------------------------------------------------------------------------------------------------------------------------------+------------------------+
|                               | **Parameter Definitions in asynNDArrayDriver.h and EPICS Record Definitions in NDArrayBase.template (file-related records are in NDFile.template)**|                        |
+-------------------------------+--------------------+---------------------------------------------------------------------------+---------------------------------------------------+------------------------+
|                               |                    | **Information about the version of ADCore and the plugin or driver**      |                                                   |                        |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| **Parameter index variable**  | **asyn interface** | **Access**  | **Description**                                             | **drvInfo string** |  **EPICS record name**       |  **EPICS record type** |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| NDADCoreVersion               | asynOctet          | r/o         | ADCore version number. This can be used by                  |                    |                              |                        |
|                               |                    |             | Channel Access clients to alter their behavior              | ADCORE_VERSION     | $(P)$(R)ADCoreVersion_RBV    |     stringin           |
|                               |                    |             | depending on the version of ADCore that was used            |                    |                              |                        |
|                               |                    |             | to build this driver or plugin.                             |                    |                              |                        |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| NDDriverVersion               | asynOctet          | r/o         | Driver or plugin version number. This can be used by        |                    |                              |                        |
|                               |                    |             | Channel Access clients to alter their behavior              | DRIVER_VERSION     | $(P)$(R)DriverVersion_RBV    |     stringin           |
|                               |                    |             | depending on the version of the plugin or driver.           |                    |                              |                        |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
|                               |                    |  **Information about the asyn port**                                      |                                                   |                        |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| NDPortNameSelf                | asynOctet          | r/o         | Aasyn port name                                             | PORT_NAME_SELF     | $(P)$(R)PortName_RBV         |     stringin           |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
|                               |                    | **Data Type**                                                             |                                                   |                        |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| NDDataType                    | asynInt32          | r/w         | Data type (NDDataType_t).                                   | DATA_TYPE          | $(P)$(R)DataType             | mbbo                   |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
|                               |                    |             |                                                             |                    | $(P)$(R)DataType_RBV         | mbbi                   |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
|                               |                    |  **Color Mode**                                                           |                                                   |                        |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| NDColorMode                   | asynInt32          | r/w         | Color mode (NDColorMode_t).                                 | COLOR_MODE         | $(P)$(R)ColorMode            | mbbo                   |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
|                               |                    |             |                                                             |                    | $(P)$(R)ColorMode_RBV        | mbbi                   |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+
| NDBayerPattern                | asynInt32          | r/o         | Bayer pattern (NDBayerPattern_t) of NDArray data.           | BAYER_PATTERN      | $(P)$(R)BayerPattern_RBV     | mbbi                   |
+-------------------------------+--------------------+-------------+-------------------------------------------------------------+--------------------+------------------------------+------------------------+

