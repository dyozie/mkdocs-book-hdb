---
title: TRUNCATE
---

<!--
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

Empties a table of all rows.

## Synopsis<a id="topic1__section2"></a>

``` pre
TRUNCATE [TABLE] <name> [, ...] [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`TRUNCATE` quickly removes all rows from a table or set of tables.This is most useful on large tables.

## Parameters<a id="topic1__section4"></a>

<dt> \<name\>   </dt>
<dd>Required. The name (optionally schema-qualified) of a table to be truncated.</dd>

<dt>CASCADE  </dt>
<dd>Since this key word applies to foreign key references (which are not supported in HAWQ) it has no effect.</dd>

<dt>RESTRICT  </dt>
<dd>Since this key word applies to foreign key references (which are not supported in HAWQ) it has no effect.</dd>

## Notes<a id="topic1__section5"></a>

Only the owner of a table may `TRUNCATE` it. `TRUNCATE` will not perform the following:

-   Run any user-defined `ON DELETE` triggers that might exist for the tables.

    **Note:** HAWQ does not support user-defined triggers.

-   Truncate any tables that inherit from the named table. Only the named table is truncated, not its child tables.

## Examples<a id="topic1__section6"></a>

Empty the table `films`:

``` sql
TRUNCATE films;
```

## Compatibility<a id="topic1__section7"></a>

There is no `TRUNCATE` command in the SQL standard.

## See Also<a id="topic1__section8"></a>

[DROP TABLE](DROP-TABLE/index.html)
