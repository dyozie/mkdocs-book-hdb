---
title: DEALLOCATE
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

Deallocates a prepared statement.

## Synopsis<a id="topic1__section2"></a>

``` pre
DEALLOCATE [PREPARE] <name>
         
```

## Description<a id="topic1__section3"></a>

`DEALLOCATE` is used to deallocate a previously prepared SQL statement. If you do not explicitly deallocate a prepared statement, it is deallocated when the session ends.

For more information on prepared statements, see [PREPARE](PREPARE.html).

## Parameters<a id="topic1__section4"></a>

<dt>PREPARE  </dt>
<dd>Optional key word which is ignored.</dd>

<dt>\<name\>  </dt>
<dd>The name of the prepared statement to deallocate.</dd>

## Examples<a id="topic1__section5"></a>

Deallocated the previously prepared statement named `insert_names`:

``` pre
DEALLOCATE insert_names;
```

## Compatibility<a id="topic1__section6"></a>

The SQL standard includes a `DEALLOCATE` statement, but it is only for use in embedded SQL.

## See Also<a id="topic1__section7"></a>

[EXECUTE](EXECUTE.html), [PREPARE](PREPARE.html)
