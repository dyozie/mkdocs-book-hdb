---
title: DROP GROUP
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

Removes a database role.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP GROUP [IF EXISTS] <name> [, ...]
```

## Description<a id="topic1__section3"></a>

`DROP GROUP` is an obsolete command, though still accepted for backwards compatibility. Groups (and users) have been superseded by the more general concept of roles. See [DROP ROLE](DROP-ROLE/index.html) for more information.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the role does not exist. A notice is issued in this case.</dd>

<dt>\<name\>   </dt>
<dd>The name of an existing role.</dd>

## Compatibility<a id="topic1__section5"></a>

There is no `DROP GROUP` statement in the SQL standard.

## See Also<a id="topic1__section6"></a>

[DROP ROLE](DROP-ROLE/index.html)
