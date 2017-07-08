NDArray
=======

The NDArray (N-Dimensional array) is the class that is used for passing detector data from drivers to plugins. An NDArray is a general purpose class for handling array data. An NDArray object is self-describing, meaning it contains enough information to describe the data itself. It can optionally contain "attributes" (class NDAttribute) which contain meta-data describing how the data was collected, etc.

An NDArray can have up to ND_ARRAY_MAX_DIMS dimensions, currently 10. A fixed maximum number of dimensions is used to significantly simplify the code compared to unlimited number of dimensions. Each dimension of the array is described by an `NDDimension structure <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/struct_n_d_dimension.html>`_. The `NDArray class documentation <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/class_n_d_array.html>`_ describes this class in detail.
