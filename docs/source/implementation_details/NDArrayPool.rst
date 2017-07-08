NDArrayPool
===========

The NDArrayPool class manages a free list (pool) of NDArray objects. Drivers allocate NDArray objects from the pool, and pass these objects to plugins. Plugins increase the reference count on the object when they place the object on their queue, and decrease the reference count when they are done processing the array. When the reference count reaches 0 again the NDArray object is placed back on the free list. This mechanism minimizes the copying of array data in plugins. The `NDArrayPool class documentation <http://cars.uchicago.edu/software/epics/areaDetectorDoxygenHTML/class_n_d_array_pool.html>`_ describes this class in detail.
