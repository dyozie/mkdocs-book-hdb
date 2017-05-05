---
title: DROP CAST
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

Removes a cast.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP CAST [IF EXISTS] (<sourcetype> AS <targettype>) [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`DROP CAST` will delete a previously defined cast. To be able to drop a cast, you must own the source or the target data type. These are the same privileges that are required to create a cast.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS </dt>
<dd>Do not throw an error if the cast does not exist. A notice is issued in this case.</dd>

<dt>\<sourcetype\> </dt>
<dd>The name of the source data type of the cast to be removed.</dd>

<dt>\<targettype\> </dt>
<dd>The name of the target data type of the cast to be removed.</dd>

<dt>CASCADE</dt>
<dt>RESTRICT</dt>
<dd>These keywords have no effect since there are no dependencies on casts.</dd>

## Examples<a id="topic1__section5"></a>

To drop the cast from type `text` to type `int`:

``` pre
DROP CAST (text AS int);
```

## Compatibility<a id="topic1__section6"></a>

The `DROP CAST` command conforms to the SQL standard.

## See Also<a id="topic1__section7"></a>

[CREATE CAST](CREATE-CAST/index.html)


