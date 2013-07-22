<!--
-*- mode: markdown; coding: utf-8; fill-column: 78 -*-
-->

## HBase Data Types (WIP)

Nick Dimiduk
Member of Technical Staff, HBase
[HBase Birds of a Feather](http://www.meetup.com/hbaseusergroup/events/119154442/),
[Hadoop Summit 2013](http://hadoopsummit.org/san-jose/)

<!-- SLIDE -->

## motivation

 - `byte[]` is necessary but not sufficient for building applications
 - every application must (re-)solve it's own subset of the
   "serialization problem"
 - ecosystem tools must agree in order to interoperate
 - application code full of pre/post client API-call serialization
   litter

<!-- SLIDE -->

## design

 1. think holistically -- not just Java
 1. encoding format that preserves non-encoded order
 1. user-extensible API for defining types
 1. provide basic data types out of the box
 1. extend client API with type support
 1. extend MapReduce API with type support

<!-- SLIDE -->

## tricky bits

 - encoding format => data on disk => painful to change
 - respect encoding performance overhead in tight loops
 - user extensible, user usable API
   - realistic API that real apps can build against
   - don't impost on POJOs (no forced interfaces, &c)
   - avoid magic (no ASM, no AOP, avoid
     [ORM scorn](http://java.dzone.com/articles/martin-fowler-orm-hate))
 - community agreement on what data types to ship out of the box
 - more stuff I haven't {thought of,encountered} yet

<!-- SLIDE -->

## inspiration

 - order-preserving encoding strategies:
   [SQLite4](http://sqlite.org/src4/doc/trunk/www/key_encoding.wiki),
   [Orderly](https://github.com/ndimiduk/orderly),
   [Phoenix](https://github.com/forcedotcom/phoenix)
 - user extensible data types:
   [PostgreSQL](http://www.postgresql.org/docs/9.2/static/sql-createtype.html)
 - (SQL) data types and API supporting execution engines
   [Hive](http://hive.apache.org/), [Pig](http://pig.apache.org/),
   [Phoenix](https://github.com/forcedotcom/phoenix),
   [Impala](https://github.com/cloudera/impala)
 - Experiments with client APIs:
   [Typo](https://github.com/keith-turner/typo)

<!-- SLIDE -->

## implementation

 - [HBASE-8089](https://issues.apache.org/jira/browse/HBASE-8089): Add
   type support (*parent ticket*)
 - [HBASE-8201](https://issues.apache.org/jira/browse/HBASE-8201):
   Implement serialization strategies (*patch available*)
 - [HBASE-8694](https://issues.apache.org/jira/browse/HBASE-8694):
   Performance evaluation of serialization strategies (*unassigned*)
 - [HBASE-8693](https://issues.apache.org/jira/browse/HBASE-8693):
   Implement extensible type API based on serialization primitives
   (*WIP*)
 - [HBASE-7941](https://issues.apache.org/jira/browse/HBASE-7941):
   Provide client API with support for primitive types (*unassigned*)
 - [HBASE-8593](https://issues.apache.org/jira/browse/HBASE-8593):
   Type support in ImportTSV tool (*WIP*)

<!-- SLIDE -->

## future directions

 - consider data types anywhere else users touch HBase
   - bundled MR jobs
   - `Filter`s
 - extend RegionServer API with type support (?!?)
   - `Coprocessor`s?
   - `StoreFile` formats?
 - non-Java implementation for non-Java clients

<!-- SLIDE -->

## thanks!
