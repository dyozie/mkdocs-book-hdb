---
title: DROP EXTERNAL TABLE
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

Removes an external table definition.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP EXTERNAL [WEB] TABLE [IF EXISTS] <name> [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`DROP EXTERNAL TABLE` drops an existing external table definition from the database system. The external data sources or files are not deleted. To execute this command you must be the owner of the external table.

## Parameters<a id="topic1__section4"></a>

<dt>WEB  </dt>
<dd>Optional keyword for dropping external web tables.</dd>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the external table does not exist. A notice is issued in this case.</dd>

<dt>\<name\>   </dt>
<dd>The name (optionally schema-qualified) of an existing external table.</dd>

<dt>CASCADE  </dt>
<dd>Automatically drop objects that depend on the external table (such as views).</dd>

<dt>RESTRICT  </dt>
<dd>Refuse to drop the external table if any objects depend on it. This is the default.</dd>

## Examples<a id="topic1__section5"></a>

Remove the external table named `staging` if it exists:

``` pre
DROP EXTERNAL TABLE IF EXISTS staging;
```

## Compatibility<a id="topic1__section6"></a>

There is no `DROP EXTERNAL TABLE` statement in the SQL standard.

## See Also<a id="topic1__section7"></a>

[CREATE EXTERNAL TABLE](CREATE-EXTERNAL-TABLE.html)
