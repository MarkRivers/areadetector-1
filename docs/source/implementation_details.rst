Implementation Details
======================

The areaDetector module depends heavily on asyn. It is the software that is used for interthread communication, using the standard asyn interfaces (e.g. asynInt32, asynOctet, etc.), and callbacks. In order to minimize the amount of redundant code in drivers, areaDetector has been implemented using C++ classes. The base classes, from which drivers and plugins are derived, take care of many of the details of asyn and other common code. 

.. toctree::

   implementation_details/asynPortDriver
   implementation_details/NDArray
   implementation_details/NDArrayPool.rst
   implementation_details/NDAttribute.rst
   implementation_details/NDAttributeList.rst
   implementation_details/PVAttribute.rst
   implementation_details/paramAttribute.rst
   implementation_details/functAttribute.rst
   implementation_details/asynNDArrayDriver.rst 

