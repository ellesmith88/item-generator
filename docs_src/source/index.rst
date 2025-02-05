.. item_generator documentation master file, created by
   sphinx-quickstart on Mon Jun  7 14:27:53 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

***************
Item Generator
***************

This library aims to be a generic tool for generating JSON documents which are `STAC <https://github.com/radiantearth/stac-spec/>`_-like.
You should be able to generate fully STAC compliant JSON or generate content which contains
all the relevant information to allow you to construct a valid `STAC item <https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md>`_.

This library works on the premise that you can build a processing chain for each of your datasets
by chaining together different processors to extract the relevant information. The core facet
extraction chain works on an atomic basis, where input plugins provide a single object for inspection
and output a single JSON object. Item IDs are generated based on selected facets. It is then
up to your downstream processing to aggregate this information together.

Datastores such as Elasticsearch can make use of upserts which will merge the JSON documents in indexing.

Installation
============

At present, not all the required libraries are available via package managers. To install, you'll
need to install the dependencies using the ``requirements.txt``

.. code-block:: console

   $ git clone https://github.com/cedadev/item-generator
   $ cd item-generator
   $ pip install -r requirements.txt
   $ pip install .

Configuration
=============

Configuration takes the form a YAML formatted file.
Configuration for the extraction pipelines is in the form of the `item description files <https://cedadev.github.io/asset-scanner/item_descriptions.html>`_. These YAML files specify
the :ref:`processors <processors>` to use.

.. list-table::
   :header-rows: 1

   * - Option
     - Description
   * - ``extractor``
     - The python import path to the extractor class. If not specified, it picks up the
       class installed with the entry point ``asset_scanner.extractors``
   * - ``item_descriptions.root_directory``
     - ``REQUIRED`` Path to the top level directory containing your dataset specific pipelines
   * - ``inputs``
     - ``REQUIRED`` Must have at least one `input plugin <https://cedadev.github.io/asset-scanner/input_plugins.html>`_.
   * - ``outputs``
     - ``REQUIRED`` Must have at least one `output plugin <https://cedadev.github.io/asset-scanner/output_plugins.html>`_

Sample configuration:
---------------------

   .. code-block:: yaml

      item_descriptions:
        root_directory: /etc/item-generator/item_descriptions/descriptions
      inputs:
        - name: file_system
          path: /badc/faam/data/2005/b069-jan-05
      outputs:
        - name: standard_out


Usage
=====

The tool is called using the `asset-scanner <https://cedadev.github.io/asset-scanner/usage.html>`_

.. program-output:: asset_scanner -h

Example:

   .. code-block:: console

      $ asset_scanner conf/conf.yml

.. toctree::
   :maxdepth: 2
   :caption: User guide:

   processors

.. toctree::
   :maxdepth: 2
   :caption: API

   core

Indices and tables
==================

* :ref:`modindex`
* :ref:`search`
