HGVS position crossmapper
=========================

.. image:: https://img.shields.io/github/last-commit/mutalyzer/crossmapper.svg
   :target: https://github.com/mutalyzer/crossmapper/graphs/commit-activity
.. image:: https://travis-ci.org/mutalyzer/crossmapper.svg?branch=master
   :target: https://travis-ci.org/mutalyzer/crossmapper
.. image:: https://readthedocs.org/projects/mutalyzer-crossmapper/badge/?version=latest
   :target: https://mutalyzer-crossmapper.readthedocs.io/en/latest
.. image:: https://img.shields.io/github/release-date/mutalyzer/crossmapper.svg
   :target: https://github.com/mutalyzer/crossmapper/releases
.. image:: https://img.shields.io/github/release/mutalyzer/crossmapper.svg
   :target: https://github.com/mutalyzer/crossmapper/releases
.. image:: https://img.shields.io/pypi/v/mutalyzer-crossmapper.svg
   :target: https://pypi.org/project/mutalyzer-crossmapper/
.. image:: https://img.shields.io/github/languages/code-size/mutalyzer/crossmapper.svg
   :target: https://github.com/mutalyzer/crossmapper
.. image:: https://img.shields.io/github/languages/count/mutalyzer/crossmapper.svg
   :target: https://github.com/mutalyzer/crossmapper
.. image:: https://img.shields.io/github/languages/top/mutalyzer/crossmapper.svg
   :target: https://github.com/mutalyzer/crossmapper
.. image:: https://img.shields.io/github/license/mutalyzer/crossmapper.svg
   :target: https://raw.githubusercontent.com/mutalyzer/crossmapper/master/LICENSE.md

----

This library provides an interface to convert (cross map) between different
HGVS numbering_ systems.

Converting between the transcript oriented c. or n. and the genomic oriented g.
numbering systems can be difficult, especially when the transcript in question
resides on the complement strand.

**Features:**

- Support for genomic positions to standard coordinates and vice versa.
- Support for noncoding positions to standard coordinates and vice versa.
- Support for coding positions to standard coordinates and vice versa.
- Support for protein positions to standard coordinates and vice versa.
- Basic classes for loci that can be used for genomic loci other than genes.

Please see ReadTheDocs_ for the latest documentation.


Quick start
-----------

The ``Crossmap`` class provides an interface to all conversions between
positioning systems.

.. code:: python

    >>> from mutalyzer_crossmapper import Crossmap

When initialised with no gene information, the only conversions available are
those between genomic positions and standard coordinates.

.. code:: python

    >>> crossmap = Crossmap()
    >>> crossmap.coordinate_to_genomic(0)
    1
    >>> crossmap.genomic_to_coordinate(1)
    0

When initialised with only exon information, conversions to and from noncoding
positions are available as well.

.. code:: python

    >>> exons = [(5, 8), (14, 20), (30, 35), (40, 44), (50, 52), (70, 72)]
    >>> crossmap = Crossmap(exons)
    >>> crossmap.coordinate_to_noncoding(35)
    (14, 1)
    >>> crossmap.noncoding_to_coordinate((14, 1))
    35

Add the flag ``inverted=True`` to the constructor when the transcript resides
on the reverse complement strand.

When initialised with both exon information as well as a CDS, conversions to
and from coding positions and protein positions are available as well.

.. code:: python

    >>> cds = (32, 43)
    >>> crossmap = Crossmap(exons, cds)
    >>> crossmap.coordinate_to_coding(31)
    (-1, 0, 0)
    >>> crossmap.coding_to_coordinate((-1, 0, 0))
    31

Again, the flag ``inverted=True`` can be used for transcripts that reside on
the reverse complement strand.


.. _numbering: http://varnomen.hgvs.org/bg-material/numbering/
.. _ReadTheDocs: https://mutalyzer-crossmapper.readthedocs.io
