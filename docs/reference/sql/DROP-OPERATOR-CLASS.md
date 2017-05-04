---
title: DROP OPERATOR CLASS
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

Removes an operator class.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP OPERATOR CLASS [IF EXISTS] <name> USING <index_method> [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`DROP OPERATOR` drops an existing operator class. To execute this command you must be the owner of the operator class.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the operator class does not exist. A notice is issued in this case.</dd>

<dt>\<name\>   </dt>
<dd>The name (optionally schema-qualified) of an existing operator class.</dd>

<dt>\<index\_method\>   </dt>
<dd>The name of the index access method the operator class is for.</dd>

<dt>CASCADE  </dt>
<dd>Automatically drop objects that depend on the operator class.</dd>

<dt>RESTRICT  </dt>
<dd>Refuse to drop the operator class if any objects depend on it. This is the default.</dd>

## Notes

This command will not succeed if there are any existing indexes that use the operator class. Add `CASCADE` to drop such indexes along with the operator class.

## Examples<a id="topic1__section5"></a>

Remove the B-tree operator class `widget_ops`:

``` pre
DROP OPERATOR CLASS widget_ops USING btree;
```

This command will not succeed if there are any existing indexes that use the operator class. Add `CASCADE` to drop such indexes along with the operator class.

## Compatibility<a id="topic1__section6"></a>

There is no `DROP OPERATOR CLASS` statement in the SQL standard.

## See Also<a id="topic1__section7"></a>

[ALTER OPERATOR](ALTER-OPERATOR.html), [CREATE OPERATOR](CREATE-OPERATOR.html) [CREATE OPERATOR CLASS](CREATE-OPERATOR-CLASS.html)
