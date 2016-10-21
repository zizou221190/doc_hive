Divers fichiers en source
=========================


.. contents::
   :local:
   :depth: 2

L'utilisation du serde
----------------------

SerDe is short for Serializer/Deserializer. Hive uses the SerDe interface for IO. The interface handles both serialization and deserialization and also interpreting the results of serialization as individual fields for processing.

A SerDe allows Hive to read in data from a table, and write it back out to HDFS in any custom format. Anyone can write their own SerDe for their own data formats.

See `Hive SerDe`_ for an introduction to SerDes.

.. _Hive SerDe: https://cwiki.apache.org/confluence/display/Hive/DeveloperGuide#DeveloperGuide-HiveSerDe


Les differents fichiers sources
-------------------------------



JSON
^^^^

To use the SerDe, specify the fully qualified class name ``org.apache.hive.hcatalog.data.JsonSerDe``, for example:

**Create table, specify CSV properies**

.. code-block:: json

    
         CREATE TABLE my_table(a string, b bigint, ...)

         ROW FORMAT SERDE 'org.apache.hive.hcatalog.data.JsonSerDe'

         STORED AS TEXTFILE;




XML
^^^



CSV
^^^


TXT
^^^


Mainframe
^^^^^^^^^

1.      Using Cobol Serde in Hive:


``Upload it in the home dir of the current user: /user/a*****/lib/cobolserde.jar``


2.      Create Data directory:


``/data/landing/files/sia_source/appli_source/``


3.      Put your binary files in this directory

4.      Create your Layout File:

``/user/a*****/layout/sia_source/appli_source/layout_name``


5.      Edit the Layout File to put the structure of the files:

        Example::

         20  BVMDOC2-VIN-ID                      PIC  X(17).
         20  BVMDOC2-NUM-DATA                   PIC  9(5) COMP-3.
         20  BVMDOC2-VAL-DATA                   PIC  X(100).




You need to respect the alignment, otherwise you will receive an error while the serde is parsing the layout

The current serde doesn’t support the Signed Definition.
For example, to define   ``PIC  S9(4) COMP-3.``    You need to write   ``PIC  9(5) COMP-3.``
**We are working on adding this possibility.**

6.      As a last step, you need to define your Hive Table:


.. code-block:: json


        ADD JAR hdfs://datalabha/user/a****/lib/cobolserde.jar;
        CREATE EXTERNAL TABLE IF NOT EXISTS dbname.tablename
        ROW FORMAT SERDE 'com.savy3.hadoop.hive.serde2.cobol.CobolSerDe'
        STORED AS
        INPUTFORMAT 'org.apache.hadoop.mapred.FixedLengthInputFormat'
        OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.IgnoreKeyTextOutputFormat'
        LOCATION '/data/landing/files/sia_source/appli_source'
        TBLPROPERTIES ('cobol.layout.url'= 'hdfs://datalabha/user/a*****/layout/sia_source/appli_source/layout_name ','fb.length'='120')


Please pay attention to the calculation of the length of the data file:
For decimal like PIC 9(n) the length is n/2 +1
``In this case the length = 17 + Int(5/2) + 1 + 100 = 17 +2 +1 + 100 = 120``

