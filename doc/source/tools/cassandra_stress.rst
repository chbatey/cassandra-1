.. Licensed to the Apache Software Foundation (ASF) under one
.. or more contributor license agreements.  See the NOTICE file
.. distributed with this work for additional information
.. regarding copyright ownership.  The ASF licenses this file
.. to you under the Apache License, Version 2.0 (the
.. "License"); you may not use this file except in compliance
.. with the License.  You may obtain a copy of the License at
..
..     http://www.apache.org/licenses/LICENSE-2.0
..
.. Unless required by applicable law or agreed to in writing, software
.. distributed under the License is distributed on an "AS IS" BASIS,
.. WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
.. See the License for the specific language governing permissions and
.. limitations under the License.

.. highlight:: yaml

.. _cassandra_stress:

Cassandra Stress
----------------

cassandra-stress is a tool for benchmarking and load testing a Cassandra
cluster. cassandra-stress supports testing arbitrary CQL tables and queries
to allow users to benchmark their data model.

This documentation focuses on user mode as this allows the testing of your
actual schema. 

Usage
^^^^^
There are several operation types:

    * write-only, read-only, and mixed workloads of standard data
    * write-only and read-only workloads for counter columns
    * user configured workloads, running custom queries on custom schemas

The syntax is `cassandra-stress <command> [options]`. If you want more information on a given command
or options, just run `cassandra-stress help <command|option>`.

Commands:
    read:
        Multiple concurrent reads - the cluster must first be populated by a write test
    write:
        Multiple concurrent writes against the cluster
    mixed:
        Interleaving of any basic commands, with configurable ratio and distribution - the cluster must first be populated by a write test
    counter_write:
        Multiple concurrent updates of counters.
    counter_read:
        Multiple concurrent reads of counters. The cluster must first be populated by a counterwrite test.
    user:
        Interleaving of user provided queries, with configurable ratio and distribution.
    help:
        Print help for a command or option
    print:
        Inspect the output of a distribution definition
    legacy:
        Legacy support mode

Primary Options:
    -pop:
        Population distribution and intra-partition visit order
    -insert:
        Insert specific options relating to various methods for batching and splitting partition updates
    -col:
        Column details such as size and count distribution, data generator, names, comparator and if super columns should be used
    -rate:
        Thread count, rate limit or automatic mode (default is auto)
    -mode:
        Thrift or CQL with options
    -errors:
        How to handle errors when encountered during stress
    -sample:
        Specify the number of samples to collect for measuring latency
    -schema:
        Replication settings, compression, compaction, etc.
    -node:
        Nodes to connect to
    -log:
        Where to log progress to, and the interval at which to do it
    -transport:
        Custom transport factories
    -port:
        The port to connect to cassandra nodes on
    -sendto:
        Specify a stress server to send this command to
    -graph:
        Graph recorded metrics
    -tokenrange:
        Token range settings


Suboptions:
    Every command and primary option has its own collection of suboptions. These are too numerous to list here.
    For information on the suboptions for each command or option, please use the help command,
    `cassandra-stress help <command|option>`.

User mode
^^^^^^^^^

User mode allows you to use your stress your own schemas. This can save time in
the long run rather than building an application and then realising your schema
doesn't scale.

Profile
+++++++

User mode requires a profile defined in YAML. 

The keyspace for the test::

   keyspace: yourkeyspace

CQL for the keyspace. Optional if the keyspace already exists::

  keyspace_definition: |
    CREATE KEYSPACE yourkeyspace WITH replicatin = {'class': 'SimpleStrategy', 'replication_factor' : 1};

The table to be stressed::
  
  table: yourtable


CQL for the table. Optional if the table already exists::

  table_definition: |
    CREATE TABLE simple.kv (
      key text PRIMARY KEY,
      value text
    )

Optional metadata for each column in the above table:





Graphing
^^^^^^^^

