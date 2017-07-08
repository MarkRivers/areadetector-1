NDAttributeList
===============

The NDAttributeList implements a linked list of NDAttribute objects. NDArray objects contain an NDAttributeList which is how attributes are associated with an NDArray. There are methods to add, delete and search for NDAttribute objects in an NDAttributeList. Each attribute in the list must have a unique name, which is case-sensitive.

When NDArrays are copied with the NDArrayPool methods the attribute list is also copied.

IMPORTANT NOTE: When a new NDArray is allocated using NDArrayPool::alloc() the behavior of any existing attribute list on the NDArray taken from the pool is determined by the value of the global variable eraseNDAttributes. By default the value of this variable is 0. This means that when a new NDArray is allocated from the pool its attribute list is **not** cleared. This greatly improves efficiency in the normal case where attributes for a given driver are defined once at initialization and never deleted. (The attribute **values** may of course be changing.) It eliminates allocating and deallocating attribute memory each time an array is obtained from the pool. It is still possible to add new attributes to the array, but any existing attributes will continue to exist even if they are ostensibly cleared e.g. asynNDArrayDriver::readNDAttributesFile() is called again. If it is desired to eliminate all existing attributes from NDArrays each time a new one is allocated then the global variable eraseNDAttributes should be set to 1. This can be done at the iocsh prompt with the command:

    ``var eraseNDAttributes 1``
    

The `NDAttributeList class documentation <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/class_n_d_attribute_list.html>`_ describes this class in detail. 

 
