---
title: DROP VIEW
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

Removes a view.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP VIEW [IF EXISTS] <name. [, ...] [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`DROP VIEW` will remove an existing view. Only the owner of a view can remove it.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the view does not exist. A notice is issued in this case.</dd>

<dt>\<name\>  </dt>
<dd>The name (optionally schema-qualified) of the view to remove.</dd>

<dt>CASCADE  </dt>
<dd>Automatically drop objects that depend on the view (such as other views).</dd>

<dt>RESTRICT  </dt>
<dd>Refuse to drop the view if any objects depend on it. This is the default.</dd>

## Examples<a id="topic1__section5"></a>

Remove the view `topten`;

``` pre
DROP VIEW topten;
```

## Compatibility<a id="topic1__section6"></a>

`DROP VIEW` is fully conforming with the SQL standard, except that the standard only allows one view to be dropped per command. Also, the `IF           EXISTS` option is a HAWQ extension.

## See Also<a id="topic1__section7"></a>

[CREATE VIEW](CREATE-VIEW.html)
