---
title: DROP TABLESPACE
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

Removes a tablespace.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP TABLESPACE [IF EXISTS] <tablespacename>
         
```

## Description<a id="topic1__section3"></a>

`DROP TABLESPACE` removes a tablespace from the system.

A tablespace can only be dropped by its owner or a superuser. The tablespace must be empty of all database objects before it can be dropped. It is possible that objects in other databases may still reside in the tablespace even if no objects in the current database are using the tablespace.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the tablespace does not exist. A notice is issued in this case.</dd>

<dt>\<tablespacename\>  </dt>
<dd>The name of the tablespace to remove.</dd>

## Examples<a id="topic1__section5"></a>

Remove the tablespace `mystuff`:

``` pre
DROP TABLESPACE mystuff;
```

## Compatibility<a id="topic1__section6"></a>

`DROP TABLESPACE` is a HAWQ extension.

## See Also<a id="topic1__section7"></a>

[CREATE TABLESPACE](CREATE-TABLESPACE.html), [ALTER TABLESPACE](ALTER-TABLESPACE.html)
