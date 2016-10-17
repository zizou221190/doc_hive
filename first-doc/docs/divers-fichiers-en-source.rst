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

To use the SerDe, specify the fully qualified class name `org.apache.hive.hcatalog.data.JsonSerDe`, for example:

** Create table, specify CSV properies**

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

