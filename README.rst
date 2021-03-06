######################################################################
rdechecker: Validate & archive Real-Driving-Emissions files.
######################################################################

:release:       0.0.1
:date:          2017-12-20 08:00:00
:home:          https://github.com/JRCSTU/rdechecker/
:keywords:      rde, real-driving-emissions, vehicle-emissions, file-format, validation
:copyright:     2017 European Commission (`JRC <https://ec.europa.eu/jrc/>`_)
:license:       `EUPL 1.1+ <https://joinup.ec.europa.eu/software/page/eupl>`_

*rdechecker* validates files generated from Real-Driving-Emissions cycle tests,
before those files are submitted to monitoring bodies.
Optionally, archive and compress those files in a single HDF5 archive.

Quickstart
==========
Clone git repo and install in *develop* mode to experiment with sample files::

    git clone git+https://github.com/JRCSTU/rdechecker  rdechecker.git
    cd rdechecker.git/
    pip install -e .

(alternatively use: ``pip install git+https://github.com/JRCSTU/rdechecker.git``).

Run sample files (assumes git cloned locally, above)::

    $ cd rdechecker/tests/
    $ rdechek  f1:Sample_Data_Exchange_File.csv f2:Sample_Reporting_File_1.csv


Validations
===========
Currently partial validations are defined only for 2 file "kinds"::

    $ rdechek -l
    f1: Big file
    f2: The summary file

The validations are configured in the ``/rdechecker/tests/files-schema.yaml`` file.
The most important configuration is in ``file_kinds.sections.lines`` dictionary,
keyed by the line-number (1-based).  For example::

                1: [TEST ID, '[code]']
                2: [Test date, '[dd.mm.yyyy]', 're:\d\d.\d\d.\d{4}']
                16: [Engine rated power, '[kW]', 'float:']

Which means that:

- line 1 must have at least 2 "cells", with the exact contents shown.
- line 2 in addition must have a 3rd cell that satisfy a "date" regular-expression.
- line 16  must have 2 fixed-string cells and a float 3rd one.

Some care is needed when using the characters `:[]` because they have
special meaning in *YAML*.

Note that file ``f2`` does not specify *sections*.