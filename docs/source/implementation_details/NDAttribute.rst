NDAttribute
===========

The NDAttribute is a class for linking metadata to an NDArray. An NDattribute has a name, description, data type, value, source type and source information. Attributes are identified by their names, which are case-sensitive. There are methods to set and get the information for an attribute.

It is useful to define some conventions for attribute names, so that plugins or data analysis programs can look for a specific attribute. The following are the attribute conventions used in current plugins:


+--------------------------------------------------------------+
|          Conventions for standard attribute names            |
+===============+================+=============================+
|Attribute name |  Description   |       Data type             |
+---------------+----------------+-----------------------------+
|  ColorMode    | "Color mode"   |  	int (NDColorMode_t)    |
+---------------+----------------+-----------------------------+
| BayerPattern  |"Bayer pattern" |      int (NDBayerPattern_t) | 
+---------------+----------------+-----------------------------+


Attribute names are case-sensitive. For attributes not in this table a good convention would be to use the corresponding driver parameter without the leading ND or AD, and with the first character of every "word" of the name starting with upper case. For example, the standard attribute name for ADManufacturer should be "Manufacturer", ADNumExposures should be "NumExposures", etc.

The `NDAttribute class documentation <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/class_n_d_attribute.html>`_ describes this class in detail. 
