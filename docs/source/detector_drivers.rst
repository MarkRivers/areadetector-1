Detector Drivers
================

Here is an example of linking accross repository documentation.

- **drvInfo string:** The string used to look up the parameter in the driver through the drvUser interface. This string is used in the EPICS database file for generic asyn device support to associate a record with a particular parameter. It is also used to associate a `paramAttribute <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/classparam_attribute.html>`_ with a driver parameter in the XML file that is read by asynNDArrayDriver::readNDAttributesFile   
- **EPICS record name:** The name of the record in ADBase.template. Each record name begins with the two macro parameters $(P) and $(R). In the case of read/write parameters there are normally two records, one for writing the value, and a second, ending in _RBV, that contains the actual value (Read Back Value) of the parameter.



-  `Andor <http://areadetector.readthedocs.io/projects/adandor/en/latest/ >`_ 
-  `Andor3 <http://areadetector.readthedocs.io/projects/adandor3/en/latest/ >`_ 


