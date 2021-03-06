---
title: DROP CONVERSION
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

Removes a CONVERSION.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP CONVERSION [IF EXISTS] <name> [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`DROP CONVERSION` removes a previously defined conversion. To be able to drop a conversion, you must own the conversion.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the conversion does not exist. A notice is issued in this case.</dd>

<dt>\<name\>   </dt>
<dd>The name of the conversion.  The conversion name may be schema-qualified.</dd>

<dt>CASCADE</dt>
<dt>RESTRICT</dt>
<dd>These keywords have no effect since there are no dependencies on conversions.</dd>

## Examples<a id="topic1__section5"></a>

Drop the conversion named `myname`:

``` pre
DROP CONVERSION myname;
```

## Compatibility<a id="topic1__section6"></a>

There is no `DROP CONVERSION` statement in the SQL standard.

## See Also<a id="topic1__section7"></a>

[CREATE CONVERSION](CREATE-CONVERSION.html), [ALTER CONVERSION](ALTER-CONVERSION/index.html)


