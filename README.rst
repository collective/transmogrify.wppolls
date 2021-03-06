********************
transmogrify.wppolls
********************

.. contents:: Table of Contents

Life, the Universe, and Everything
==================================

Transmogrifier pipeline sections to import WordPress polls created with the `WP-Polls`_ plugin into Plone.
This package depends on `collective.polls`_.

.. _`collective.polls`: https://pypi.python.org/pypi/collective.polls
.. _`WP-Polls`: https://wordpress.org/plugins/wp-polls/

Mostly Harmless
===============

.. image:: http://img.shields.io/pypi/v/transmogrify.wppolls.svg
    :target: https://pypi.python.org/pypi/transmogrify.wppolls

.. image:: https://img.shields.io/travis/collective/transmogrify.wppolls/master.svg
    :target: http://travis-ci.org/collective/transmogrify.wppolls

.. image:: https://img.shields.io/coveralls/collective/transmogrify.wppolls/master.svg
    :target: https://coveralls.io/r/collective/transmogrify.wppolls

Got an idea? Found a bug? Let us know by `opening a support ticket`_.

.. _`opening a support ticket`: https://github.com/collective/transmogrify.wppolls/issues

Don't Panic
===========

Installation
------------

To enable this package in a buildout-based installation:

#. Edit your buildout.cfg and add add the following to it::

    [buildout]
    ...
    eggs =
        transmogrify.wppolls

After updating the configuration you need to run ''bin/buildout'', which will
take care of updating your system.

Usage
-----

1. Export as CSV the following tables from your WordPress site usign the phpMyAdmin interface:

- wp_pollsa
- wp_pollsq

Use the following options for all:

- Fields terminated by '\\t'
- Remove CRLF characters within fields
- Put field names in the first row

.. figure:: https://raw.github.com/collective/transmogrify.wppolls/master/export.png
    :align: center
    :height: 465px
    :width: 600px

    Export from phpMyAdmin as CSV.

2. Edit the 'wppolls.cfg' file and set the options: 'source', 'path', 'locale' and 'transitions'
3. On the ZMI, got to 'portal_setup/Import' and select the profile 'transmogrify.wppolls'
4. Click on 'Import all steps' and wait for the process to finish

Sections
========

transmogrify.wppolls.csvsource
------------------------------

This source section generates a list of polls and its results;
it doesn't takes care of any information about the voters.

.. code-block:: ini

    [csvsource]
    blueprint = transmogrify.wppolls.csvsource
    source = /home/customer/site/data/
    path = /polls
    locale = pt-br
    transitions = open

source:
    full path to the WordPress CSV export files.

path:
    path, relative to the site root, of the folder where polls should be created;
    defaults to '/polls'.

locale:
    if you want the id normalizer to be aware of locale; defaults to 'en'.

transitions:
    a sequence of workflow transition names that will be executed.
    By default, polls are opened and then closed ('open, close').

transmogrify.wppolls.voteupdater
---------------------------------

This section is needed to update the results of a poll as the schema updater section doesn't know how to deal with it.
It must be used after the constructor.

.. code-block:: ini

    [voteupdater]
    blueprint = transmogrify.wppolls.voteupdater
