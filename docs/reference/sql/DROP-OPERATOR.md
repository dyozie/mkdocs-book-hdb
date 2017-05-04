---
title: DROP OPERATOR
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

Removes an operator.

## Synopsis<a id="topic1__section2"></a>

``` pre
DROP OPERATOR [IF EXISTS] <name> ( {<lefttype> | NONE} , 
    {<righttype> | NONE} ) [CASCADE | RESTRICT]
```

## Description<a id="topic1__section3"></a>

`DROP OPERATOR` drops an existing operator from the database system. To execute this command you must be the owner of the operator.

## Parameters<a id="topic1__section4"></a>

<dt>IF EXISTS  </dt>
<dd>Do not throw an error if the operator does not exist. A notice is issued in this case.</dd>

<dt>\<name\>   </dt>
<dd>The name (optionally schema-qualified) of an existing operator.</dd>

<dt>\<lefttype\>  </dt>
<dd>The data type of the operator's left operand; write `NONE` if the operator has no left operand.</dd>

<dt>\<righttype\> </dt>
<dd>The data type of the operator's right operand; write `NONE` if the operator has no right operand.</dd>

<dt>CASCADE  </dt>
<dd>Automatically drop objects that depend on the operator.</dd>

<dt>RESTRICT  </dt>
<dd>Refuse to drop the operator if any objects depend on it. This is the default.</dd>

## Examples<a id="topic1__section5"></a>

Remove the power operator `a^b` for type `integer`:

``` pre
DROP OPERATOR ^ (integer, integer);
```

Remove the left unary bitwise complement operator `~b` for type `bit`:

``` pre
DROP OPERATOR ~ (none, bit);
```

Remove the right unary factorial operator `x!` for type `bigint`:

``` pre
DROP OPERATOR ! (bigint, none);
```

## Compatibility<a id="topic1__section6"></a>

There is no `DROP OPERATOR` statement in the SQL standard.

## See Also<a id="topic1__section7"></a>

[ALTER OPERATOR](ALTER-OPERATOR.html), [CREATE OPERATOR](CREATE-OPERATOR.html)
