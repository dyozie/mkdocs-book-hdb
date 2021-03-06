---
title: DROP FILESPACE
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

Removes a filespace.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP FILESPACE [IF EXISTS]  <filespacename>
         
```

## Description<a id="topic1__section3"></a>

`DROP FILESPACE` removes a filespace definition and its system-generated data directories from the system.

A filespace can only be dropped by its owner or a superuser. The filespace must be empty of all tablespace objects before it can be dropped. It is possible that tablespaces in other databases may still be using a filespace even if no tablespaces in the current database are using the filespace.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the filespace does not exist. A notice is issued in this case.</dd>

<dt>\<filespacename>   </dt>
<dd>The name of the filespace to remove.</dd>

## Examples<a id="topic1__section5"></a>

Remove the tablespace `myfs`:

``` pre
DROP FILESPACE myfs;
```

## Compatibility<a id="topic1__section6"></a>

There is no `DROP FILESPACE` statement in the SQL standard or in PostgreSQL.

## See Also<a id="topic1__section7"></a>

[DROP TABLESPACE](DROP-TABLESPACE/index.html), [hawq filespace](../cli/admin_utilities/hawqfilespace.html#topic1)
