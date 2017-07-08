========
Overview
========

The areaDetector module provides a general-purpose interface for area (2-D) detectors in EPICS. It is intended to be used with a wide variety of detectors and cameras, ranging from high frame rate CCD and CMOS cameras, pixel-array detectors such as the Pilatus, and large format detectors like the Perkin Elmer flat panels.

The goals of this module are: 

- Minimize the amount of code that needs to be written to implement a new detector.
- Provide a standard interface defining the functions and parameters that a detector driver must support.
- Provide a set of base EPICS records that will be present for every detector using this module. This allows the use of generic EPICS clients for displaying images and controlling cameras and detectors.
- Allow easy extensibility to take advantage of detector-specific features beyond the standard parameters.
- Have high-performance. Applications can be written to get the detector image data through EPICS, but an interface is also available to receive the detector data at a lower-level for very high performance.
- Provide a mechanism for device-independent real-time data analysis such as regions-of-interest and statistics.
- Provide detector drivers for commonly used detectors in synchrotron applications. These include Prosilica GigE video cameras, IEEE 1394 (Firewire) cameras, ADSC and MAR CCD x-ray detectors, MAR-345 online imaging plate detectors, the Pilatus   pixel-array detector, Roper Scientific CCD cameras, Perkin-Elmer amorphous silicon detector, and many others.

.. contents:: Contents:
   :local:

