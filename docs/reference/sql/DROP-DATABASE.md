---
title: DROP DATABASE
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

Removes a database.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP DATABASE [IF EXISTS] <name>
         
```

## Description<a id="topic1__section3"></a>

`DROP DATABASE` drops a database. It removes the catalog entries for the database and deletes the directory containing the data. It can only be executed by the database owner. Also, it cannot be executed while you or anyone else are connected to the target database. (Connect to `template1` or any other database to issue this command.)

**Warning:** `DROP DATABASE` cannot be undone. Use it with care!

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the database does not exist. A notice is issued in this case.</dd>

<dt>\<name\>   </dt>
<dd>The name of the database to remove.</dd>

## Notes<a id="topic1__section5"></a>

`DROP DATABASE` cannot be executed inside a transaction block.

This command cannot be executed while connected to the target database. Thus, it might be more convenient to use the program `dropdb` instead, which is a wrapper around this command.

## Examples<a id="topic1__section6"></a>

Drop the database named `testdb`:

``` pre
DROP DATABASE testdb;
```

## Compatibility<a id="topic1__section7"></a>

There is no `DROP DATABASE` statement in the SQL standard.

## See Also<a id="topic1__section8"></a>

[CREATE DATABASE](CREATE-DATABASE/index.html)
