.. image:: https://zenodo.org/badge/DOI/10.5281/zenodo.213475.svg
   :target: https://doi.org/10.5281/zenodo.213475
   
.. image:: https://travis-ci.org/scivision/lowtran.svg?branch=master
    :target: https://travis-ci.org/scivision/lowtran
    
.. image:: https://coveralls.io/repos/github/scivision/lowtran/badge.svg?branch=master
    :target: https://coveralls.io/github/scivision/lowtran?branch=master
    
.. image:: https://ci.appveyor.com/api/projects/status/85epbcxvbgxnkp62?svg=true
    :target: https://ci.appveyor.com/project/scivision/lowtran

.. image:: https://api.codeclimate.com/v1/badges/fb6bf9d0351130bba583/maintainability
   :target: https://codeclimate.com/github/scivision/lowtran/maintainability
   :alt: Maintainability

=======
Lowtran
=======
LOWTRAN7 atmospheric absportion extinction model.
Updated by Michael Hirsch to be platform independent and easily accessible from Python.

The main LOWTRAN program has been made accessible from Python by using direct memory transfers instead of the cumbersome and error-prone process of writing/reading text files.
`xarray.Dataset` high-performance N-D array data is passed out, with all approprirate metadata.

:Python API Author: Michael Hirsch, Ph.D.
:License: MIT

.. contents::


Gallery
=======
See below for how to make these examples.

.. image:: gfx/lowtran.png
    :alt: "Lowtran7 absorption"
    :scale: 25 %
    
   
   

Install
=======
`See this page if you have errors on Fortran compilation. <https://www.scivision.co/f2py-running-fortran-code-in-python-on-windows>`_

* Linux: ``apt install gfortran``    
* Mac: ``brew install gcc``
* `Windows <https://www.scivision.co/windows-gcc-gfortran-cmake-make-install/>`_

If you don't already have Numpy, ``python -m pip install numpy`` and then::

  python -m pip install -e .

Examples
========

In these examples, you can write to HDF5 with the ``-o`` option.
If you have unused (singleton) dimensions, use `.squeeze()` on the output array.

We present examples of:

* ground-to-space transmittance: ``TransmittanceGround2Space.py``

  .. image:: gfx/txgnd2space.png
    :alt: "Lowtran7 transmission"
    :scale: 25 %
    
* sun-to-observer scattered radiance (why the sky is blue): ``ScatterRadiance.py``

  .. image:: gfx/whyskyisblue.png
    :alt: "Lowtran7 Scatter Radiance"
    :scale: 25 %

* sun-to-observer irradiance: ``SolarIrradiance.py``

  .. image:: gfx/irradiance.png
    :alt: "Lowtran7 Solar Irradiance"
    :scale: 25 %

* observer-to-observer solar single-scattering solar radiance (up-going) with custom Pressure, Temperature and partial pressure for 12 species ``UserDataHorizontalRadiance.py``

  .. image:: gfx/thermalradiance.png
    :alt: "Lowtran7 Solar Radiance"
    :scale: 25 %
    
    
* observer-to-observer transmittance with custom Pressure, Temperature and partial pressure for 12 species ``UserDataHorizontalTransmittance.py``
* observer-to-observer transmittance ``HorizontalTransmittance.py``

  .. image:: gfx/horizcompare.png
    :alt: "Lowtran7 Horizontal Path transmittance"
    :scale: 25 %
    
Notes
=====
`LOWTRAN7 User manual <http://www.dtic.mil/dtic/tr/fulltext/u2/a206773.pdf>`_ -- you may refer to this to understand what parameters I've set to default. Currently I don't have any aerosols enabled for example, though it's trivial to add.

Right now a lot of configuration features aren't implemented, please request those you want.

Reference
~~~~~~~~~

`Original 1994 Lowtran7 Code <http://www1.ncdc.noaa.gov/pub/data/software/lowtran/>`_

* ``LOWFIL`` program in reference/lowtran7.10.f was not connected as I had previously implemented my own filter function directly in Python.
* ``LOWSCAN`` spectral sampling (scanning) program in reference/lowtran7.13.f was not connected as I had no need for coarser spectral resolution.

Fortran (optional)
~~~~~~~~~~~~~~~~~~
This is not necessary for normal users::

    cd bin
    cmake ..
    make
    make test

should generate `this text output <https://gist.github.com/scienceopen/89ef2060d8f15b0a60914d13a61e33ab>`_.


Windows f2py
~~~~~~~~~~~~
(this is handled automatically by ``setup.py``, noted here for debugging)

Yes, even though you're `using a 64-bit compiler <https://scivision.co/f2py-running-fortran-code-in-python-on-windows/>`_::

    f2py --compiler=mingw32 -m lowtran7 -c lowtran7.f

Tested on Windows with `MinGW <https://sourceforge.net/projects/mingw-w64/>`_.

Windows Fortran compile
~~~~~~~~~~~~~~~~~~~~~~~
Normal users don't need to do this. I suggest that you instead use Cygwin or Windows Subsytem for Linux::

    cd bin
    cmake -G "MinGW Makefiles" ..
    make
    make test
